## KEYSTONE
```
controller 노드에서 여러 서비스 사용에 필요한 인증 서비스를 통합 관리한다.

 ※Swift만 사용하는 경우, Swift는 TempAuth를 제공하여 독자적인 서비스가 가능하기 때문에 굳이 설치하지 않아도 상관없다.
```
* [keystone install docs](https://docs.openstack.org/keystone/rocky/install/keystone-install-ubuntu.html#configure-the-apache-http-server)
---
### Install and configure

- keystone을 위해 사용할 database 를 만들고, 접근을 위한 설정을 한다. 
- 필요한 패키지 설치
  - keystone : openstack 인증 서비스 component
  - apache2 : HTTP server를 제공하는 아파치 재단의 프로젝트 중 하나
  - libapache2-mod-wsgi : python2로 쓰인 web application과 web server의 Web Server Gateway Interface를 제공하는 아파치 모듈(libapache2-mod-wsgi-py3 : python3버전)
- 사용하는 토큰 제공자 : fernet(기존 UUID는 스토리지 공간 문제, 네트워크 트래픽 문제, 이후 PKI/PKIZ는 큰 공유 캐시 문제와 UUID보다 느린 성능으로 제거됨, https://cyuu.tistory.com/143)
- Bootstrap the Identity service  
v2 API에서 admin을 위한 port (35357)을 별개로 두어 5000번과 35357을 두가지를 사용했으나, 
v3 API를 사용하게 되며 5000번으로 통일해서 사용한다.
일부 가이드에서 수정이 제대로 이루어지지 않아 35357을 사용하므로 주의
35357을 사용할 경우 추후 사용자 인증 과정에서 communication error가 발생 할 수 있다.
- client 환경 변수와 커맨드 옵션 사용 / [환경 script(OpenRC file) 이용](https://docs.openstack.org/keystone/rocky/install/keystone-openrc-ubuntu.html)

### Create a domain, projects, users, and roles
[Identity service (API v3) entity 구조](https://docs.openstack.org/keystone/pike/admin/identity-concepts.html)
- domain: 관리 바운더리. 하나의 회사, operator owned space 등 인증을 관리하는 큰 범위
  - project(=tenant): group
    - user: 사용자
  - user: 사용자
role: 서비스를 이용할 수 있는 권한을 나타냄. user와 project에 지정가능하며, user는 project마다 다른 role을 가지거나, 같은 project 내에서 여러 role을 가질 수 있다.(서비스마다 conf 설정을 통해 특정 operation 사용 가능 role을 설정한다. > user가 서비스를 요청 > 서비스는 해당 유저의 role을 식별 > role에 맞는 operation 권한 부여)

### Verify operation
### Create OpenStack client environment scripts
