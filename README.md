## Install_Openstack

Openstack을 사용해 storage cloud service 환경을 구축하는 방법에 대해 설명한다.


* ['얼구름' 프로젝트](https://github.com/KwakSeungeun/MediaProject)를 기준으로 작성하였다. 

* 설치 과정은 Openstack [공식 설치 Docs](https://docs.openstack.org/rocky/install/)와 [TheSkillPedia](https://www.youtube.com/playlist?list=PLbPTI55vtiGvUxCA8sObjkoqJvRh06OlZ) 유튜브 채널을 참고했다.

* Openstack을 설치하며 설치한 패키지, 설정 값의 의미와 설치 중 이슈와 해결 방법 등을 작성하였다. 

---

### Environment

* Ubuntu version : 18.04 server LTS
* Openstack version : rocky
* node : controller, storage1, storage2
* used component : keystone, swift

---

### 설치 중 이슈


