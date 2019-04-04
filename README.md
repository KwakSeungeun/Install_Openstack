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
