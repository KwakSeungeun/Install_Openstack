### KEYSTONE
```
controller 노드에서 여러 서비스 사용에 필요한 인증 서비스를 통합 관리한다.

 ※Swift만 사용하는 경우, Swift는 TempAuth를 제공하여 독자적인 서비스가 가능하기 때문에 굳이 설치하지 않아도 상관없다.
```
---

#### DB, Apache HTTP 서버 설정
```
keystone을 위해 사용할 database 를 만들고, 접근을 위한 설정을 한다. 

※ Bootstrap the Identity service 단계
v2 API에서 admin을 위한 port (35357)을 별개로 두어 5000번과 35357을 두가지를 사용했으나, v3 API를 사용하게 되며 5000번으로 통일해서 사용한다.
일부 가이드에서 수정이 제대로 이루어지지 않아 35357을 사용하므로 주의
35357을 사용할 경우 추후 사용자 인증 과정에서 communication error가 발생 할 수 있다.

※ admin으로 서비스를 사용하기 위한 계정 설정 과정이다.
소개하는 것처럼 command로 하나하나 값을 export하는 것이 아닌, script로 작성하는 방법도 있다. 
```
* [script를 사용한 계정설정](https://docs.openstack.org/keystone/rocky/install/keystone-openrc-ubuntu.html)
* [keystone 설치 가이드](https://docs.openstack.org/keystone/rocky/install/keystone-install-ubuntu.html#configure-the-apache-http-server)

---

#### Create a domain, projects, users, and roles
```
domain: 
project:
user:
role:
https://docs.openstack.org/keystone/rocky/install/keystone-users-ubuntu.html
```
