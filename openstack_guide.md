<h3>오픈스택 설치 가이드</h3>  
※ 우리 프로젝트를 기준으로 작성하였다.  
※ openstack 공식 설치 가이드를 기준으로 하되, 겪은 시행착오와 Tip, 설치 구성 요소의 의미를 중심으로 작성하였다.  
※ Ubuntu 18.04 server LTS, openstack rocky 버전을 사용함.
(https://docs.openstack.org/keystone/rocky/install/keystone-install-ubuntu.html)    
※ 설치 가이드 링크에서 command의 시작이 $: 유저 모드, #: sudo 모드
※ 가이드에서 제시하는 설치 대상이 어떤 노드인지 확인하고 진행하자 
※ 컴포넌트 설치 및 설정에 사용하는 비밀번호 분실주의

<h4>PREREQUISITES</h4>
<h5>Host networking</h5>
노드마다 네트워크 인터페이스를 설정하고 노드의 privateIP주소에 hostname을 등록하는 과정
https://docs.openstack.org/install-guide/environment-networking.html
※Configure network interface 키워드 의미
https://unix.stackexchange.com/questions/128439/good-detailed-explanation-of-etc-network-interfaces-syntax

<h5>NTP</h5>
노드간 synchronization을 위한 과정
controller 노드는 정확한 time server를 reference하고, 
그 외 노드들이 controller 노드를 reference 하도록 설정한다.
https://docs.openstack.org/install-guide/environment-ntp.html

<h5>Openstack package install</h5>
오픈스택의 최신 버전을 수시로 받기 위해 PPA(Personal Package Archive)를 이용한다.
add-apt-repository를 통해 PPA를 추가하고 패키지 최신 업데이트와 설치한다.
https://docs.openstack.org/install-guide/environment-packages-ubuntu.html

<h5>DB, Message Queue, Memcached & etcd 설치</h5>
오픈스택 서비스 컴포넌트들은 정보 저장을 위한 데이터베이스와 operation과 정보 처리를 위해 message queue를 미들웨어로 사용한다.
인증 서비스(keystone)에서 토큰 캐시 저장을 위해 Memcached 를 사용한다. 클라우드 서비스 deployment를 위해서는 firewalling , authentication, encryption을 권장한다.
etcd는 분산 key-value store
etcd는 네트워크로 연결된 노드들 중 리더를 선정해서 클러스터를 관리한다. 데이터는 분산 저장되기 때문에 리더를 포함한 시스템의 오류에 대한 내성을가진다.
응용 프로그램은 etcd에 데이터를 읽고 쓸 수 있다. 간단한 예제는 데이터베이스 연결 정보와 플래그 값을 etcd에 저장하는 것이다. etcd는 값 변경에 대한 감시 기능을 제공하므로 설정이 변경될 경우 앱을 재구성할 수있다. 좀 더 나아가면 데이터에 대한 일관성을 보장하는 점을 활용해서 리더를 선출하고, 작업 클러스터 전체에 대한 분산 잠금을 수행 할 수 있다.
https://www.joinc.co.kr/w/man/12/etcd
이들은 대개 컨트롤러 노드에서 동작한다.
https://docs.openstack.org/install-guide/environment-sql-database-ubuntu.html
https://docs.openstack.org/install-guide/environment-messaging-ubuntu.html
https://docs.openstack.org/install-guide/environment-memcached-ubuntu.html
https://docs.openstack.org/install-guide/environment-etcd-ubuntu.html

<h4>KEYSTONE</h4>
controller 노드에서 여러 서비스 사용에 필요한 인증 서비스를 통합 관리한다.
※Swift만 사용하는 경우, Swift는 TempAuth를 제공하여 독자적인 서비스가 가능하기 때문에 굳이 설치하지 않아도 상관없다.

<h5>DB, Apache HTTP 서버 설정</h5>
keystone을 위해 사용할 database 를 만들고, 접근을 위한 설정을 한다. 
※ Bootstrap the Identity service 단계 
v2 API에서 admin을 위한 port (35357)을 별개로 두어 5000번과 35357을 두가지를 사용했으나, v3 API를 사용하게 되며 5000번으로 통일해서 사용한다.
일부 가이드에서 수정이 제대로 이루어지지 않아 35357을 사용하므로 주의, 35357을 사용할 경우 추후 사용자 인증 과정에서 communication error가 발생 할 수 있다.
※ admin으로 서비스를 사용하기 위한 계정 설정 과정이다.
소개하는 것처럼 command로 하나하나 값을 export하는 것이 아닌, script로 작성하는 방법도 있다. 
일반 유저 설정에도 동일한 방법을 적용하면 편한다. > https://docs.openstack.org/keystone/rocky/install/keystone-openrc-ubuntu.html
https://docs.openstack.org/keystone/rocky/install/keystone-install-ubuntu.html#configure-the-apache-http-server

<h5>Create a domain, projects, users, and roles</h5>
domain: 
project:
user:
role:
https://docs.openstack.org/keystone/rocky/install/keystone-users-ubuntu.html


<h5></h5>

<h5></h5>


<h4>KEYSTONE</h4>  
Keystone은 openstack 서비스를 이용하기 위한 인증과 관리 등을 제공하는 서비스이다.  





<h4>linux command</h4>

파일시스템확인
df -T

설치된 패키지 리스트
apt list --installed


<h2>openstack setting guide</h2>
ec2 인스턴스에 파일시스템 추가방법
새로운 EBS를 인스턴스에 추가하고 연결 > 해당 EBS 파일시스템 설정 > 인스턴스와 EBS 마운트 > /etc/fstab 설정에 디바이스 등록해서 재부팅할 때마다 연결된 EBS 볼륨을 탑재하도록 함

<h2>conf 링크 주소</h2>
>> vim /etc/swift/account, container, object-server.conf >>
https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/account-server.conf-sample
https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/container-server.conf-sample
https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/object-server.conf-sample

>>  /etc/swift/swift.conf >> 
https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/swift.conf-sample

>> /etc/swift/proxy-server.conf >>
https://opendev.org/openstack/swift/raw/branch/stable/rocky/etc/proxy-server.conf-sample


ec2 인스턴스간 통신 > 각각의 인스턴스가 포함된 보안그룹의 인바운드 규칙 수정 > 모든icmp ipv4 > 통신을 원하는 인스턴스가 속한 보안 그룹 추가 ( 두 인스턴스가 같은 보안 그룹에 속해있는 경우 인바운드 규칙에 해당 보안 그룹을 포함해주어야 통신 가능 > ping 확인


ls -l 파일 권한 정보
https://conory.com/blog/19194

<scp를 이용한 인스턴스간 파일 전송>

scp -i pem파일(디렉토리 포함) / 전송할 파일 / 원격지 유저이름@ip주소(public):~/저장위치
(-i 옵션: Specifies an alternate identification file to use for public key authentication. )
(https://stackoverflow.com/questions/11388014/using-scp-to-copy-a-file-to-amazon-ec2-instance)

>>원격지의 다운로드 위치의 소유자와 유저이름이 일치해야 함 > chown 
>>permission denied > 디렉토리 권한 확인 ex)700?????

(apt-get update : 사용 가능한 패키지들과 그 버전들의 리스트를 업데이트 하는 명령어. 
 실제 패키지 버전을 업그레이드하는 것이 아니라 최신 버전 패키지가 있는지를 확인하고 내 우분투에 알려주는 용도.)
(apt-get upgrade : 내 우분투에 있는 패키지들을 실제로 최신 버전으로 업그레이드 하는 명령어. 
여기서 최신 버전이란 위의 apt-get update 명령어를 수행했을 때 확인된 최신 버전이겠지?)
(참고: https://dowhateveryouwant.tistory.com/11)



openstack glossary : https://docs.openstack.org/doc-contrib-guide/common/glossary.html
