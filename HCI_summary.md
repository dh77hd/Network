# HCI

## # HyperFlex - Cisco HCI

### 1. 특징

- DCB 트래픽 제어
- 실시간 분산형 3중 복제 : 데이터를 모든 노드에 동시에 분산시키고 신속한 쓰기 작업을 위해 SSD를 캐시로 사용
- Compute Only Node 제공
- 로그 스트럭처 파일시스템
- 64노드 클러스터(32 HX + 32 Computing Only)
- LAZ(Logical Availability Zone)
  - HX 노드 그룹을 Logical Availability Group으로 생성
  - HXDP는 동일 LAZ에 데이터 2 카피를 저장하지 X
  - LAZ 적용 클러스터는 데이터 손실이나 가용성 손실 없이 동시 2개 이상 노드 장애 견딜 수 있음



### 2. 인터사이트

- 차세대 UCS 관리(기존 UCSM, UCS Director 등 통합)
- 클라우드 기반
- 서버 펌웨어 정보 표시



### 3. 내부 소프트웨어 모듈

![HX_1](C:\Users\LDCC\Desktop\Git\Note\HX_1.PNG)

- CVM : 드라이브 직접 액세스
- VAAI : 클론과 스냅샷 운영을 간단하게 통합
- IOVisor : 하이퍼바이저에 NFS 형태로 표시하고 IO 분산



### 4. HX 3.5

- All NVMe 지원
- 하드웨어 엑셀러레이션 지원(PCIe 카드 제공)
- GPU for AI/ML
- Hyper-V 지원
- 컨테이너 에코 시스템(OpenShift 지원)
- 업그레이드 강화(No vMotion)
- 인터사이트를 통한 스토리지 분석 기능 제공