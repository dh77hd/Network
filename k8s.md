# #Kubernetes, k8s

## 1. 개요

- 쿠버네티스는 배포 자동화, 스케일링, 컨테이너화된 애플리케이션의 관리를 위한 오픈소스 시스템
- 구글에 의해 설계되었고, 현재 리눅스 재단 CNCF에 의해 관리
- 여러 클러스터의 호스트 간에 애플리케이션 컨테이너의 배치, 스케일링, 운영을 자동화하기 위한 플랫폼 제공
- 도커를 포함한 일련의 컨테이너 도구들과 함께 동작



### 1-1. 구성

##### 기본 구성

HA 가능 마스터노드와 실제 앱이 구동하는 컨테이너들의 Pod를 호스팅하는 1개 이상의 워커 노드로 클러스터 구성

[이미지1]

### 1-2. 데이터센터 OS

##### 데이터센터 OS

- 데이터센터는 하나의 큰 컴퓨터
- 데이터센터가 하나의 큰 컴퓨터가 되기 위해 데이터 센터 내의 많은 CPU, 메모리, 저장장치, 네트워킹 등의 세부사항들을 추상화하는 운영체제
- 쿠버네티스는 데이터센터 운영체제가 되기 위한 것 중 하나

![1533101892267](C:\Users\LDCC\Desktop\Git\Note\image\k8s_01.PNG)

### 1-3. 특징

- Automatic Bin-Packing : 가용성을 희생하지 않는 범위 안에서 리소스를 충분히 활용해서 컨테이너를 배치
- Self-Healing : 컨테이너 실행 실패, Node가 죽거나 반응이 없는 경우, Health check에 실패한 경우에 해당 컨테이너 서비스를 자동 복구
- Horizontal Scaling : Pod의 CPU 사용이나 App이 제공하는 Metric을 기반으로 ReplicaSet의 컨테이너를 스케줄링하여 성능 제어
- Service discovery and load balancing : Application 수정 없는 Service Discovery 메커니즘을 위해 Container에 고유 IP, 단일 DNS 이름을 제공하여 로드밸런싱
- Automated rollouts and rollbacks : application, configuration의 변경 시, 서비스의 중단없이 점진적으로 Container를 변경(Rolling, Update)하고 문제 발생시 Rollback
- Secret and configuration management : 보안키, configuration을 이미지의 변경없이 업데이트할 수 있고, 노출(expose)하지 않고 관리 사용
- Storage orchestration : local storage를 비롯해서 public cloud, network storage 등을 원하는 구성으로 자동 mount
- Batch Execution : batch, CI 작업의 수행을 관리할 수 있으며, 실패 시 컨테이너를 교체하는 것도 가능



## 2. 아키텍처

### 2-1. 아키텍처 구성

- 마스터노드와 1개 이상의 워커 노드(Worker Node)로 이뤄진 가상/물리 머신의 Set를 통해 클러스터 구성
- 마스터 노드  HA : Kube-scheduler와 kube-controller-manager는 마스터의 HA 구성에서도 1개만 Active
- Nodes @ Dashboard : k8s 클러스터를 위한 웹 UI 상의 node
- Pods @ Dashboard : k8s 클러스터를 위한 웹 UI 상의 노드별 동작 기능
- Nodes @ ntopng : k8s 생성 플로우와 사용 프로토콜
- Flow @ ntopng : k8s 노드 간 활성 플로우 모니터
- 마스터 노드
  - etcd : 클러스터의 상태 저장 / k8s API object 저장 / Persistent Data Volume을 위한 SSD 권장
  - API server
  - Controller manager server
  - Scheduler
- 워커 노드
  - Kubelet : agent 로서 API server에 연결하여 데이터/볼륨/이미지/컨테이너 상태를 처리하며 Health check도 제공
  - Kube-proxy : 외부 접속을 위한 서비스 라우팅과 로드밸런싱을 처리하고 iptable을 사용
  - Ingress Controller : L7 기반 로드밸런싱 지원

### 2-2. 컨트롤러 매니저

- Kubernetes controller manager (KCM)에서 Cloud controller manager (CCM)을 분리하여 별도의 프로세스로 구동하여 클라우드에 의존적이던 KCM 환경을 개선
- KCM
  - Node Controller
  - Volume Controller
  - Route Controller
  - Service Controller
- CCM
  - Node Controller
  - Route Controller
  - Service Controller

### 2-3. kube scheduler(스케줄러)

- Kube scheduler : 새로 생성한 Pod가 구동 하는 노드를 선택하고 감시하는 마스터의 구성 요소이며, 스케줄링의 결정을 위해 자원 요구, 하드웨어/소프트웨어/정책 억제, 친화와 비친화의 명시, 데이터의 위치, 워크로드 간 간섭과 데드라인 요소 수집
- 스케줄링 : 컨테이너가 실행 할 노드를 결정하며, CPU/메모리/동작 중인 컨테이너 수를 기반으로 고려하여 배치
- 스케줄링 방법
  - 예측 가능 : 노드의 자원이나 성격으로 지정하며, Pod의 포트, Pod의 자원, 노드 지정 매치, 호스트 이름, 서비스 친화, 라벨
  - 가중치 우선 순위 지정 : 가중치에 최적인 노드를 사용

### 2-4. kube proxy (프록시)

- kube-proxy : Pod 간 Node 간 통신을 지원

### 2-5. API 서버

- API server(kube-apiserver) : 대시보드와 CLI 등은 모두 API 서버를 통해 k8s를 연결 하며, API를 이용하는 프로그램 개발이 가능
- 쿠버네티스 요소들의 허브로, HTTP, HTTPS 기반의 RESTful API 제공

### 2-6. etcd 서버

- etcd(Distributed key-value store) : Configuration, Namespace, Replication 등의 pod/service 등의 상태 저장과 DNS 데이터 저장에 사용
- 쿠버네티스 클러스터의 많은 서비스가 etcd에 의존하며, 직접 접속하거나 kube-apiserver를 통함

### 2-7. 컨테이너 런타임

- 구성 요소의 도커 컨테이너화(Dockerizing) : k8s 마스터와 워크 노드들은 컨테이너화 한 구성 요소들과 프로세스로 동작

### 2-8. kubelet (에이전트)

- kubelet : 각 노드에서 구동하는 에이전트로 마스터에서 Pod의 정보를 제공. 마스터/워커 노드 모두에서 실행하고 Health check 기능도 포함

### 2-9. POD

- k8s의 동작을 위한 단위이며, 하나 이상의 컨테이너들로 구성하여 동일 IP/port로 localhost나 프로세스간 통신을 함. 컨테이너들은 스토리지를 공유
- 배치의 가장 작은 컨테이너 묶음 단위로서, Pod 내부의 컨테이너들은 테넌트 구성이 가능한 network namespace와 data volume을 공유하며 클러스터링 구성



## 3. K8s 네트워킹

### 3-1. CNI

- CNI(Container Network Interface) : 런타임과 플러그인 간의 상호작용을 정의

  - 런타임과 독립 : 컨테이너 네트워킹을 위해서 런타임에 독립적인 플랫폼
  - 지원 OS : CoreOS, OpenShift, Mesos, Cloud Foundry, K8s, Kurma, rkt

- Plugin 구성

  - IPAM : 할당된 IP를 유지하고 로컬 데이터베이스
  - VETH Pair : 자원들 간 필요한 연결의 논리적 링크 형성
  - 기존 네트워크 요소 필요

- Plugin 종류

  | 이름 | 제조사 | 내용 |
  | ---- | ------ | ---- |
  | ACI  |        |      |

  







[향 후 추가]

엔터프라이즈 컨테이너 솔루션 찾기

자동화된 배포, 버전업 관리 필수

비교표



빅데이터 서비스











