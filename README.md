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

### 개발 중 이슈
- 초기에 private cloud를 개발하기 위해 개인 PC에서 노드를 구성하기를 계획했고, Oracle VirtualBox로 4개의 instance로 노드를 구성해서 개발을 진행했다.
- 로컬의 인스턴스를 이용할 경우 외부에서 해당 인스턴스에 접근하기 위해서 _**포트 포워딩**_ 방식을 이용해서 처리했다.
- 하지만 개발을 진행할수록 _**PC성능의 한계**_ 로 컴퓨터가 멈추는 현상이 잦아졌고, 노트북을 이용하여 학교나 카페에서 팀 협업을 하는 경우 공용 공유기에 접근이 차단되어 포트 포워딩으로 로컬 인스턴스로의 접근이 불가능했다. 이런 이유로 원활한 개발이 어려웠다.
- 결국 _**AWS EC2 instance**_ 로 노드를 구성하기로 했다. public cloud의 인스턴스로 private cloud를 개발하는 꼴이 되었지만, 진행을 위해서 어쩔 수 없다고 판단했다.
- ec2 instance를 사용하면서 공식 설치 가이드, 유튜브 가이드 영상을 따라서 진행하는 것과 차이가 발생했다. 
- _**노드간 네트워킹**_, _**노드간 파일 전송**_, 그리고 _**노드에 storage device 추가**_ 등의 과정을 진행할 때, 그 차이로 인한 에러들이 발생하여 이들을 해결하고 진행해야 했다. 이런 차이는 AWS에서 _**ec2 instance의 네트워킹 보안**_ 과 _**storage device 추가 방식**_ 의 정책 차이에 의한 것으로 _**인스턴스의 인바운드 규칙 추가**_ 와 _**EBS 볼륨 추가로 storage device 추가**_ 등의 방법으로 이런 이슈를 해결했다. 
자세한 그 해결 과정은 KEYSTONE과 SWIFT 설치 가이드를 통해 확인할 수 있다.
---
- [WHAT IS CLOUD 바로가기](https://github.com/KwakSeungeun/Install_Openstack/tree/master/What%20_is_CLOUD)
- [INSTALL OPENSTACK 바로가기](https://github.com/KwakSeungeun/Install_Openstack/tree/master/Install_OEPNSTACK)
  - [KEYSTONE 설치 가이드 바로가기](https://github.com/KwakSeungeun/Install_Openstack/blob/master/Install_OEPNSTACK/KEYSTONE.md)
  - [SWIFT 설치 가이드 바로가기](https://github.com/KwakSeungeun/Install_Openstack/blob/master/Install_OEPNSTACK/SWIFT.md)
