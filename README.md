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

1. 디스크 2개 추가 (sdb, sdc)
```
* EC2를 사용할 경우 Elastic block storage > 블록 에서 디스크 추가 후 storage node연결해야함
* VM을 사용할 경우 디스크 두개를 
```

2. rsync 설치 : 다량의 파일들을 전송/수신하고, 데이터 증분치에 대한 반영을 할 수 있는 가장 좋은 방법

출처: https://www.yongbok.net/blog/tag/%EC%9A%B0%EB%B6%84%ED%88%AC-rsync-%EC%84%A4%EC%A0%95/ [nota's story]

2-1 설치
```
apt-get install xinetd rsync
```

2-2 vi /etc/xinetd.d/rsync (rsync service 등록)
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
