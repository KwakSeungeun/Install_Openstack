# Install_Openstack


## Environment

* Ubuntu version : 18.04
* Openstack version : rocky
* node : controller, storage1, storage2
* usig component : keystone, swift

---

## Install controller node


---

## Install storage1 node
![Swift structure](https://tino1b2be.com/wp-content/uploads/2017/01/state-of-the-stack-april-2013-50-638.jpg)

### controller node 설정 (controller node가 swift-proxy 역할을 하게 됨)


### storage node 설정

* 디스크 2개 추가 (sdb, sdc)
```
* EC2를 사용할 경우 Elastic block storage > 블록 에서 디스크 추가 후 storage node연결해야함
* VM을 사용할 경우 디스크 두개를 
```

* rsync 설치 : 다량의 파일들을 전송/수신하고, 데이터 증분치에 대한 반영을 할 수 있는 가장 좋은 방법 ![자료](https://www.yongbok.net/blog/tag/%EC%9A%B0%EB%B6%84%ED%88%AC-rsync-%EC%84%A4%EC%A0%95/)

1. 설치
```
apt-get install xinetd rsync
```

2. vi /etc/xinetd.d/rsync (rsync service 등록)
```
service rsync
{
disable = no
socket_type = stream
wait = no
user = root
server = /usr/bin/rsync
server_args = –daemon
log_on_failure += USERID
}
```

3. vi /etc/rsyncd.conf
```

uid = swift
gid = swift
log file = /var/log/rsyncd.log
pid file = /var/run/rsyncd.pid
address = #이곳에 접근 할 수 있는 IP주소

[account]
max connections = 2
path = /srv/node/
read only = False
lock file = /var/lock/account.lock

[container]
max connections = 2
path = /srv/node/
read only = False
lock file = /var/lock/container.lock

[object]
max connections = 2
path = /srv/node/
read only = False
lock file = /var/lock/object.lock

```

4. rysnc service 시작
```
systemctl enable rsync
systemctl start rsync
```

### storage node ring 설정

1. account.builder file 생성
```
swift-ring-builder account.builder create 10 3 1
```

2. 각 storage node에 ring연결
```
swift-ring-builder account.builder \
  add --region 1 --zone 1 --ip 자기자신privateIP --port 6202 \
  --device 이곳에 더 붙여준 디스크이름 --weight 그 디스크 사이즈

# ( EXAMPLE )
# EC2에서 볼륨을 추가 할 경우 xvdb형태로 이름이 자동으로 변경
swift-ring-builder account.builder add \
  --region 1 --zone 1 --ip 172.31.0.0 --port 6202 --device xvdb --weight 10
swift-ring-builder account.builder add \
  --region 1 --zone 1 --ip 172.31.0.0 --port 6202 --device xvdc --weight 10
  
# account 연결 확인 명령어
swift-ring-builder account.builder
```

