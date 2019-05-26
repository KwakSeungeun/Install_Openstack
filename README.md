# Install_Openstack

Openstack을 사용해 storage cloud service 환경을 구축하는 방법에 대해 설명한다.


* ['얼구름' 프로젝트](https://github.com/KwakSeungeun/MediaProject)를 기준으로 작성하였다. 

* Openstack  [공식 설치 가이드](https://docs.openstack.org/rocky/install/)를 기준, 겪은 시행착오와 Tip, 설치 구성 요소의 의미를 중심으로 작성하였다. 

* Version : Ubuntu 18.04 server LTS, openstack rocky

* 설치 가이드 링크에서 command의 시작이 $: 유저 모드, #: sudo 모드 

* 가이드에서 제시하는 설치 대상이 어떤 노드인지 확인하고 진행하자 

* 컴포넌트 설치 및 설정에 사용하는 비밀번호 분실주의

---

## Environment

* Ubuntu version : 18.04
* Openstack version : rocky
* node : controller, storage1, storage2
* usig component : keystone, swift
