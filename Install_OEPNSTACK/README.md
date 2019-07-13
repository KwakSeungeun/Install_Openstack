## PREREQUISITES
- [install guide](https://docs.openstack.org/install-guide/)
---
### Overview
#### Architecture
```
Controller : 인증 서비스, 이미지 서비스 등의 서비스와 노드간 네트워킹, 요청 메세지 처리, NTP 등 컨트롤러로써의 역할 수행 
Compute : 가상 인스턴스를 작동시키는 가상화, hypervisor 역할과 인스턴스의 virtual network 연결, 보안 그룹을 통한 firewalling 서비스 등을 제공
Block Storage : Optional 노드, block storage로써 사용될 disk를 포함.
Object Storage : Optional 노드, object storage로써 사용될 disk를 포함.
```
[install-guide-overview](https://docs.openstack.org/install-guide/overview.html#example-architecture)

- 공식 가이드 예시 구성
![HARDWARE ARCHITECTURE](https://docs.openstack.org/install-guide/_images/hwreqs.png)

- 유튜버 영상 예시 구성
  - Controller
    - 1CPU
    - 4GB RAM
    - 10GB Storage
  - Compute
    - 1CPU
    - 1GB RAM
    - 10GB + 3GBx2 Storage
  - Storage x2
    - 1CPU
    - 1GB RAM
    - 10GB + 3GBx2 Storage  

[노드 HW 구성/네트워크 등 참고](https://www.youtube.com/watch?v=AQ8igcN2p18&list=PLbPTI55vtiGvUxCA8sObjkoqJvRh06OlZ&index=4)

- 우리 프로젝트 구성
  - Controller
    - aws ec2 t2.medium
    - EBS 10GB
  - Storage x2
    - aws ecc2 t2.micro
    - EBS 10GB
    - EBS 10GB x2 추가
    
###Environment
#### Host networking
```
노드마다 네트워크 인터페이스를 설정하고 노드의 privateIP주소에 hostname을 등록하는 과정
```
- [Configure network interface 키워드 의미](https://unix.stackexchange.com/questions/128439/good-detailed-explanation-of-etc-network-interfaces-syntax)
- AWS ec2로 노드를 구성한 경우  
노드간의 네트워킹을 위해서 instance의 보안 그룹의 인스턴스마다의 인바운드 규칙에서 상호간에 ip를 통신 가능하도록 추가해준 뒤, 위 과정을 진행해야 한다. 추가한 뒤 ping을 보내어 연결되었는지 확인한다.
##### ip 용어와 개념
- fixed ip
    - 클라우드에서 : 클라우드 플랫폼 내에서 생성된 instance에게 초기에 할당된 private ip, fixed ip로 외부망과의 통신이 불가능하고, instance간의 통신이 가능하다.  
    - 통상적으로 : ISP로부터 지정 받은 고정 ip  

- floating ip : 클라우드 플랫폼이 등장하며 생긴 개념, 외부망과의 통신이 가능하다. instance에 필요에 따라 할당과 해제가 가능하다.  
- public ip : 외부망과 통신 가능한 공인된 ip  
- dynamic ip : DHCP 프로토콜에 의해 동적으로 할당받은 ip  
#### NTP
```
노드간 synchronization을 위한 과정
controller 노드는 정확한 time server를 reference하고, 
그 외 노드들이 controller 노드를 reference 하도록 설정한다.
```
#### Openstack package install
```
오픈스택의 최신 버전을 수시로 받기 위해 PPA(Personal Package Archive)를 이용한다.
add-apt-repository를 통해 PPA를 추가하고 패키지 최신 업데이트와 설치한다.
```
#### SQL database
```
오픈스택 서비스 컴포넌트들은 정보 저장을 위해 데이터베이스를 사용한다.
```
#### Message queue
```
service를 위한 operation과 정보 처리를 위해 message queue를 미들웨어로 사용한다.
```
#### Memcached
```
인증 서비스(keystone)에서 토큰 캐시 저장을 비롯해서 데이터와 객체들을 RAM에 캐시 처리하여 속도를 높이기 위해 사용하는 범용 분산 캐시 시스템이다. 
```
#### Etcd
```
etcd는 분산 key-value store이다. 네트워크로 연결된 노드들 중 리더를 선정해서 클러스터를 관리한다. 
데이터가 분산 저장되기 때문에 리더를 포함한 시스템의 오류에 대한 내성을가진다.
응용 프로그램은 etcd에 데이터를 읽고 쓸 수 있다. 간단한 예제는 데이터베이스 연결 정보와 플래그 값을 etcd에 저장하는 것이다. 
etcd는 값 변경에 대한 감시 기능을 제공하므로 설정이 변경될 경우 앱을 재구성할 수있다. 
좀 더 나아가면 데이터에 대한 일관성을 보장하는 점을 활용해서 리더를 선출하고, 작업 클러스터 전체에 대한 분산 잠금을 수행 할 수 있다.
```
- [etcd란?](https://www.joinc.co.kr/w/man/12/etcd)
---
- [KEYSTONE 설치 바로가기](https://github.com/KwakSeungeun/Install_Openstack/blob/master/Install_OEPNSTACK/KEYSTONE.md)
- [SWIFT 설치 바로가기](https://github.com/KwakSeungeun/Install_Openstack/blob/master/Install_OEPNSTACK/SWIFT.md)
