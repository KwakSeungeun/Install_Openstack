# Install_Openstack
```
Openstack을 사용해 storage cloud service 환경을 구축하는 방법에 대해 설명한다.
Rocky version을 사용했으며, [공식문서](https://docs.openstack.org/rocky/install/)을 참고하여 작성했다.
AWS EC2에 Openstack을 구축했다.

※ 우리 프로젝트를 기준으로 작성하였다. 
※ openstack 공식 설치 가이드를 기준으로 하되, 겪은 시행착오와 Tip, 설치 구성 요소의 의미를 중심으로 작성하였다. 
※ Ubuntu 18.04 server LTS, openstack rocky 버전을 사용함. (https://docs.openstack.org/keystone/rocky/install/keystone-install-ubuntu.html) ※ 설치 가이드 링크에서 command의 시작이 $: 유저 모드, #: sudo 모드 ※ 가이드에서 제시하는 설치 대상이 어떤 노드인지 확인하고 진행하자 
※ 컴포넌트 설치 및 설정에 사용하는 비밀번호 분실주의
```

## Environment

* Ubuntu version : 18.04
* Openstack version : rocky
* node : controller, storage1, storage2
* usig component : keystone, swift
