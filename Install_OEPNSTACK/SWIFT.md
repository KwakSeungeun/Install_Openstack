## SWIFT
```
Object storage cloud service를 제공한다.
swift는 swift proxy server와 account server, container server 그리고 object server로 구성된다.  
proxy server를 제외한 server들은 ring을 통해서 관리된다. ring은 disk에 저장된 entity의 이름과 그것들의 physical location을 매핑한다.
proxy server는 요청이 있을 때, ring을 통해서 각각의 서버 위치를 확인하고 해당 요청을 적절한 서버로 전달한다. public API처리,  
object data upload, download 중에 encoding과 decoding 등을 처리한다.
object server는 object 저장, 검색, 삭제를 처리하며, 파일 메타데이터와 함께 파일 시스템에 바이너리 파일로 저장된다.
container server는 특정 container에 어떤 object들이 있는지 listing한다.
account server는 특정 account가 어떤 container를 가졌는지 listing한다.
```
* [swift architecture 자세한 설명](https://docs.openstack.org/swift/latest/overview_architecture.html)
![Swift architecture](https://image.slidesharecdn.com/swift-140701223711-phpapp02/95/openstack-swift-overview-6-638.jpg?cb=1404254496)
* [swift install docs](https://docs.openstack.org/swift/latest/install/)

---
### Install and configure the controller node
```
proxy service 설치와 설정(controller에서 proxy service를 제공하도록 설정하는 경우)
```
- ※ Swift의 경우 내부적인 인증 매커니즘이 있어 Keystone을 사용하지 않을 수도 있다.
- 필요한 패키지 설치
  - swift : openstack object storage service component
  - swift-proxy
  - python-swiftclient
  - python-keystoneclient
  - python-keystonemidlleware
  - memcached : 범용 분산 캐시 시스템  
  
- ※curl -o /etc/swift/proxy-server.conf https://opendev.org/openstack/swift/raw/branch/master/etc/proxy-server.conf-sample 명령 후 받은 conf file로 진행시 오류가 날 경우 [맨 아래 기타 참고]

### Install and configure the storage nodes
```
storage node(account, container, object) 설치와 설정
최소 2개 이상의 노드가 필요하며, 가이드에서는 XFS 파일 시스템을 사용한다.
```
#### prerequisites
- 필요한 패키지 설치
  - xfsprogs : XFS file system 관리, 디버깅 툴
  - rsync : 파일 전송과 동기화 유틸리티 프로그램
- [edit /etc/fstab](https://movenpick.tistory.com/34)  
noatime : 파일 access time을 기록하지 않아 메타 데이터 업데이트를 기록하지 않기 때문에 IO마다 오버헤드가 덜 발생해 성능 향상 효과가 있다.
다.부팅시 자동으로 마운트되도록 설정한다.(noatime을 활성화하면 nodiratime도 활성화한다. 추가로 활성화해주지 않아도 된다.)
nodiratime : 디렉토리에 대한 access time을 기록하지 않는다.
logbufs : in-memory log buffers의 크기
- 파일 시스템 준비과정  
아마존 인스턴스를 사용하는 경우 가이드대로 스토리지(sdb, sdc)에 파일 시스템을 포맷하고 마운트하려고 시도하면 에러가 발생한다.
이 경우, 아마존 EBS로 볼륨을 생성하고 인스턴스에 해당 볼륨을 장착한 뒤, EBS 볼륨을 포맷하고 마운트 해주어야 한다.
부팅시 자동 마운트 되도록 /etc/fstab으로 설정한다.
그 다음 오픈스택 설치 가이드의 순서를 다시 따르며 상황에 맞게 진행한다. 
  [위 과정 참고](http://pyrasis.com/book/TheArtOfAmazonWebServices/Chapter04/05/01)  
#### Install and configure components
- 필요한 패키지 설치
  - swift
  - swift-account
  - swift-container
  - swift-object

### Create and distribute initial rings
```
controller 노드에서 initial account, containerr, and object ring 생성하고 storage node로 배포
```
- 각 storage node에 ring연결
swift-ring-builder account.builder add --region 1 --zone 1 --ip 자기자신privateIP --port 6202 --device 이곳에 더 붙여준 디스크이름 --weight 100  
  ex : 
   swift-ring-builder account.builder add --region 1 --zone 1 --ip 172.31.0.0 --port 6202 --device xvdb --weight 100  
   swift-ring-builder account.builder add--region 1 --zone 1 --ip 172.31.0.0 --port 6202 --device xvdc --weight 100
- Ditribute ring configuration files
VirtualBox같은 가상화 프로그램을 이용해서 로컬 인스턴스로 노드를 구성한 경우, **scp [] [받을 노드의 주소 or 호스트 네임]:[받을 위치]** 명령어를 쓰면 된다.  
AWS를 이용해서 로컬 인스턴스로 구성한 경우, **scp -i [.pem파일(디렉토리path포함)] [파일명] [유저이름@인스턴스 주소]:[받을 위치]** 명령어를 쓴다.  
우리의 경우 **pem파일**을 local에서 controller 노드로 보낸 뒤 컨트롤러 노드에서 각각의 스토리지 노드로 파일을 전송했다.  
이때 인스턴스의 인바운드 규칙에 local pc의 ip와 각 노드의 ip주소를 추가하여 보안 그룹 내에 포함시켜야 한다.  
또한, 파일을 전송할 때 원격지 다운로드 위치의 소유자와 인스턴스 유저이름이 일치해야 하며, 디렉토리의 rwx 권한도 확인해줘야 한다.
(무조건 모든 권한을 허용으로 설정해도 너무 많은 권한이 허용된다는 에러 메세지와 함께 파일 전송이 되지 않으니 필요한 만큼 권한을 설정한다.)  
[scp 참고](https://stackoverflow.com/questions/11388014/using-scp-to-copy-a-file-to-amazon-ec2-instance)  
[권한 설정 참고](https://conory.com/blog/19194)

### Finalize installation
### Verify operation
---
### 기타 
[swift command](https://docs.openstack.org/ocata/cli-reference/swift.html)

[※] curl -o 로 conf file을 받아온 뒤 진행시 오류가 발생하면, 버전이 맞지 않아 그런 것이므로 아래의 주소로 받는다. 
proxy-server.conf > https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/proxy-server.conf-sample  
account-server.conf > https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/account-server.conf-sample  
container-server.conf > https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/container-server.conf-sample  
object-servre.conf > https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/object-server.conf-sample  

