# Install_Openstack


### Environment

* Ubuntu version : 18.04
* Openstack version : rocky
* node : controller, storage1, storage2
* usig component : keystone, swift

---

### Install controller node


---

### Install storage1 node

#### controller node 설정 (controller node가 swift-proxy 역할을 하게 됨)

![Swift structure](https://www.google.com/url?sa=i&source=images&cd=&cad=rja&uact=8&ved=2ahUKEwjz3_nU0LbhAhXFXLwKHT-IDw4QjRx6BAgBEAU&url=https%3A%2F%2Ftino1b2be.com%2Finstall-swift-on-a-single-vm-with-tempauth-no-openstackswiftstack-client%2F&psig=AOvVaw31JWwCfPscLaG8KtjwrxJ4&ust=1554473859062342)

1. 


#### storage node 설정

1. 디스크 2개 추가 (sdb, sdc)
```
* EC2를 사용할 경우 Elastic block storage > 블록 에서 디스크 추가 후 storage node연결해야함
* VM을 사용할 경우 디스크 두개를 
```
