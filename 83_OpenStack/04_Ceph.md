# #Ceph

SDS(Software Defined Storage) 서비스를 제공하는 솔루션

OSD : 데이터 저장, 복제, 부하분산 등의 역할 / 1TB당 메모리 1G 구성

Monitors : 클러스터 상태 체크, PG(Placement Group) map, OSD map 등 관리

MDS  :Metadata Server / 일반 사용자가 Ceph 데이터를 검색 및 체크하기 위해 metadata를 저장하는 서버



## 1. Ceph

### 1-1. Ceph란

- Ceph 는 오픈소스, 분산, 확장, 소프트웨어 정의 스토리지 시스템
- 블록, 객체 및 파일 스토리지 제공
- CRUSH 알고리즘을 사용해 중앙 집중적인 메타데이터의 필요성을 제거하고, 클러스터 내의 모든 노드에 부하를 분산
- CRUSH 알고리즘은 데이터 배치를 테이블 탐색 기반이 아닌 계산에 의해 행하며, 병목 현상의 위험성 및 연관된 단일 장애 지점 없이 확장 가능
- 클라이언트는 요청 데이터가 저장된 서버에 직접 연결되는 형태이기 때문에 데이터 경로에 중앙 집중적인 병목현상 없음
- 세 가지 주 스토리지 형태 제공
  - 블록 형태인 RADOS 블록 장치(RBD, RADOS Block Devices)
  - 파일 시스템 형태인 Ceph 파일 시스템(CephFS)
  - 객체 형태인 RADOS 게이트웨이(Reliable Autonomous Distributed Object Store)

### 1-2. 아키텍처

[이미지]

- RADOS : 실제 Data 저장을 담당
- CephFS : FUSE 지원으로 RADOS에 직접 Access 가능
- RBD : 일반적인 Block Device로 붙일 수 있도록 구성된 것으로 LIBRADOS를 통해 RADOS에 Access
- RADOSGW : LIBRADOS를 통해 object store에 접근

### 1-3. 구성요소

- ceph-mon
  - 클러스터 모니터로 Active / Failed node 를 확인하는 역할 수행
  - Ceph Storage Cluster Map의 Master copy를 유지
  - 각 클라이언트들은 모니터로부터 cluster map 정보를 가져옴
- ceph-mds
  - 메타데이터 서버
  - inode와 디렉터리들의 메타데이터를 저장
  - 클러스터 구성이 가능하여 확장 및 클러스터 호스트 사이에서의 동적인 Data 분산 가능
- ceph-osd
  - Object Storage Devices
  - 실제 파일 내용을 저장하고 OSD의 상태를 확인해서 모니터에 알려주는 역할
  - CRUSH 알고리즘을 사용하여 OSD daemon에 직접 접근
- ceph-rgw
  - RESTful Gateway
  - Object Storage Layer를 외부에 노출시키기 위한 인터페이스











### 1-5. 동작 방식

- Ceph의 핵심 스토리지 계층은 RADOS
- RADOS 계층은 여러 개의 OSD로 구성
  - 각 OSD는 완벽하게 독립적이며, peer-to-peer 관계로 클러스터 형성
  - 각 OSD는 기본 HBA(host bus adapter)를 통해 단일 물리 디스크에 매핑
- 다른 주요 구성 요소는 모니터
  - 모니터는 Paxos 사용을 통해 클러스터 쿼룸(Quorum) 형성
  - 쿼룸 형성을 통해 모니터는 클러스터를 위한 신뢰성 있는 결정을 내리고, 스플릿 브레인 시나리오를 피할 수 있는 상태에 있음을 확신
  - 데이터 경로에 직접적으로 가담하지 않고 OSD처럼 성능 조건을 요구하지 않음
  - 다양한 클러스터 맵을 사용해 멤버십, 환경설정 및 통계를 포함한 클러스터 상태 제공
  - 클러스터 맵은 Ceph 클러스터 요소와 클라이언트 모두에 의해 사용 되며, 클러스터 토폴로지를 설명하고 올바른 위치에 안전하게 데이터를 저장하기 위함
- Ceph는 PG(Placement Groups)라는 이름의 객체 그룹에 객체들을 위치시키기 위해 CRUSH를 사용
- librados는 Ceph 라이브러리로, 객체를 저장하고 추적하기 위해 RADOS 클러스터와 직접 소통하는 애플리케이션을 제작하는 데 사용



 

