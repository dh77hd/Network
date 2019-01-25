# # HCI Solutions

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

![HX_1](C:\Users\LDCC\Desktop\Git\Note\00_image\HX_1.PNG)

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



## # NETAPP HCI

   ### 1. 특징

- 고유 QoS  기능
  - VM 단위 QoS 설정 기능(VVols)
  - 어플리케이션/볼륨 당 Min, Max, Burst 성능 설정
- 유연한 확장성 : Compute, Storage Node 별로 필요한 자원만 확장 가능(1box 4node)
- vCenter 통합 관리 : 별로의 스토리지 관리툴 없이 vCenter를 통해 95% 이상의 관리 작업 가능
- SolidFire OS :  SolidFire 고유의 데이터 보호 기술 사용
- 기존  NETAPP Storage(FAS) 연동 가능 : DR로 사용
- Storage Node에서 따로 컨트롤(Compute Node에 별도의 CVM 없음)
- 2 Copy Block 방식
- 10월부터 Compute Node 에서 CPU, MEM 선택 가능

