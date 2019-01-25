# # SDN (Software Defined Network)

## 0. Software Defined

### 0-1. 소프트웨어 정의 기술

- 특정 하드웨어에 종속적이지 않으며, 소프트웨어를 사용하여 IT 인프라를 정의하고, 이를 제어관제하여 신속하고 유연한 IT 인프라를 제공하는 기술
- 소프트웨어와 하드웨어를 분리하여 하드웨어 장비에서 인프라 기능을 제공하고, 별도의 소프트웨어로 해당 장비를 운용 및 제어하는 기술

### 0-2. 소프트웨어 정의 기술 필요 이유

- IT 가 발전하고 이용자들의 니즈가 다양해지면서 비즈니스 수요에 신속히 대응할 수 있는 유연한 IT 인프라가 필요
- 공급시장에서 벤더의 독자기술에 의한 IT 인프라 구성의 경직성과 시장의 폐쇄성은 이를 저해하는 요인
- 이를 해결하기 위해 IT 인프라 장비 간 공통 API를 제공하여 시스템 호환성과 정합성을 확보하고, 공통 운용제어 플랫폼을 통한 통합 제어가 가능한 기술이 필요
- 대안 기술로 소프트웨어 정의 기술이 주목



## 1. SDN

- 네트워크를 구성하고 있는 장비들의 기능 중 Control Plane은 소프트웨어 기반 개방형 네트워크 기술을 이용한 중앙의 컨트롤러에 집중하고, 장비는 Data Plane 기능만 수행하도록 한 네트워크 기술
- 중앙의 컨트롤러는 South Bound Protocol을 통해 네트워크 장비와 통신하여 Flow Rule 등을 설정하고, North Bound API를 이용하여 Overlay, Leaf-Spine, Auto Configuration 등의 네트워크 어플리케이션을 구현
- 기존 네트워크와 비교하면, 중앙 컨트롤러에서 제공하는 API를 이용함으로써 Overlay와 같은 서비스별 가상 네트워크를 쉽게 정의할 수 있으며, 가상 머신간의 East-West 통신이 효과적으로 이루어짐

![](https://github.com/dh77hd/Network/blob/master/00_image/sdn_01.png?raw=true)

### 

## 2. Overlay SDN

### 2-1. Overlay SDN이란?

- 서버 가상화 영역에 SDN을 구현해 물리적 스위치 교체 없이 네트워크 지능화 구현
- Hypervisor 영역에 구성

[이미지]

### 2-2. 특징

- 기존 네트워크 인프라 활용
- 가상 스위치가 OpenFlow를 지원하는 방식으로 구현
- 하이퍼바이저 간을 연결하는 물리적인 네트워크 장비와는 VXLAN과 같은 터널링 기술 활용
- L2~L7 네트워크 기능 자체 제공 (ex: LB, FW, VPN ...)

### 2-3. 구성 요소

| 기능          | 역할         | 구성 요소                        |
| ------------- | ------------ | -------------------------------- |
| 컨트롤 플레인 | 경로 제어    | 컨트롤러 / 가상 라우터 컨트롤 VM |
| 데이터 플레인 | 패킷 전송    | 가상 스위치                      |
| 관리 플레인   | 설정 및 관리 | Manager                          |

- 가상 스위치 : 하이퍼바이저 커널 단에서 동작 / 터널링(VXLAN)을 사용한 오버레이 방식으로 네트워크 세그먼트 구성
- 컨트롤러 : 하이퍼바이저 내에서 스위칭과 라우팅을 제어하는 역할 

### 2-3. 주요 솔루션

- VMware **NSX**
- OpenStack **Neutron**
- Nokia **Nuage**



## 3. Underlay SDN

### 3-1. Underlay SDN이란?

- 물리적 스위치 영역에 SDN을 구현
- 어떠한 서버 가상화 환경에서도 네트워크 지능화 구현

[이미지]

### 3-2. 특징

- 네트워크 관리 및 지능화
- 자동 Configuration 구성
- 네트워크 OS 자동 설치
- HW VTEP (Leaf Switch) 에서 터널링

### 3-3. 주요 솔루션

- Cisco **ACI**
- Bigswitch **Big Cloud Fabric**



## 4. OVS (Open vSwitch)

### 4-1. 개요

- 리눅스 기반의  가상 소프트웨어 스위치
- Open source Apache 2 License
- 네트워크를 추상화하여 동적으로 제어 가능
- OpenFlow를 지원하여 SDN Switch로 사용 가능
- OVSDB 프로토콜 지원 : OVS 설정 값을 저장한 DB를 관리하기 위한 프로토콜
- 터널링 지원(VXLAN, GRE, IPSec ...)
- 모니터링 기능 지원(NetFlow, sFlow, SPAN, RSPAN ...)
- 분산 가상 스위치 기능 제공 : 다른 물리 서버에 위치한 VM을 서로 연결

### 4-2. 내부 구조

![](https://github.com/dh77hd/Network/blob/master/00_image/sdn_ovs_01.PNG?raw=true)

- ovs-ofctl : OpenFlow를 이용한 OVS 관리 툴
- ovs-dpctl : datapath 관리 툴
- ovs-appctl : vswitchd 제어를 위한 경령화된 툴
- ovs-vsctl : ovsdb-server를 이용한 vswitchd 관리 툴
- ovs-vswitchd : OVS 데몬
- openvswitch(datapath) : OVS 커널 모듈
- ovsdb-client : ovsdb-server 에 대한 CLI 제공
- ovsdb-tool : ovsdb 관리를 위한 CLI 제공
- ovsdb-server : ovsdb를 관리하기 위한 RPC 인터페이스 제공

### 4-3. 아키텍처

![](https://github.com/dh77hd/Network/blob/master/00_image/sdn_ovs_02.PNG?raw=true)

#### vswitchd

- Controller로부터 flow rule을 받아 flow table을 업데이트하고 datapath 에게 기존 flow rule을 삭제하도록 명령
- datapath 가 packet 처리를 위해 rule을 요청하면 flow rule 제공

#### datapath

- 네트워크 장비와 packet 교환
- packet을 갖고 있는 flow rule에 따라 처리
- 입력된 packet 과 매치되는 flow rule 을 갖고 있지 않은 경우, upcall 과정을 통해 flow rule을 가져옴

### 4-4. 인터페이스

![](https://github.com/dh77hd/Network/blob/master/00_image/sdn_ovs_03.PNG?raw=true)

- OVS 가 지원하는 network 제어 프로토콜을 이용(OpenFlow, OVSDB )

- Netlink를 이용해 정보 교환(Netlink : Socket을 이용한 IPC)
- Network device를 지원하는 커널 API 사용



## 5. OpenFlow

- SDN을 구현하기 위해 처음으로 제정된 표준 인터페이스

- OpenFlow 스위치, OpenFlow 컨트롤러로 구성

- Flow 정보를 제어하여 패킷의 전달 경로 및 방식 결정

  > Flow는 특정 시간 동안 네트워크 상의 지정된 관찰 지점을 지나가는 패킷의 집합
  >
  > 패킷의 출발지와 목적지 정보 등을 가진 데이터

### 5-1. 동작 방식

- OpenFlow 스위치 내부에는 패킷 전달 경로와 방식에 대한 정보를 가진 FlowTable 이 존재
- 패킷이 발생하면 FlowTable이 해당 패킷에 대한 정보를 가지고 있는지 확인
- 존재하면 그에 맞춰 패킷을 처리, 존재하지 않으면 해당 패킷에 대한 정보를 컨트롤러에 요청
- 요청을 받은 컨트롤러는 내부에 존재하는 패킷 제어 정보를 확인하고 결과를 스위치에 전달
- 컨트롤러 내의 패킷 제어 정보는 API를 통해 입력 가능
- 스위치는 컨트롤러로부터 전달 받은 제어 정보를 FlowTable에 저장하고, 동일한 패킷이 발생하면 FlowTable에 잇는 정보를 활용하여 패킷 전달

### 5-2. 패킷 제어 정보

패킷 제어 정보는 **Header Fields, Counters, Actions **로 구성

![](https://github.com/dh77hd/Network/blob/master/00_image/sdn_of_01.PNG?raw=true)

#### Header Fields

- 스위치 포트, 이더넷 및 프로토콜 정보, 출발지/목적지의 MAC, IP, 포트, 우선순위 저장
- 헤더 필드 정보와 패킷의 정보의 일치 여부에 따라, FlowTable에 존재하는 지 결정

#### Actions

- 패킷 정보가 헤더 필드 정보와 일치할 때 패킷 처리에 대한 정보
- 처리 방식은 3가지
  - 스위치에 정의되어 있는 경로에 따라 패킷 전달
  - 정해진 하나의 포트 또는 여러 개의 포트로 패킷 전달(전달 경로 변경)
  - 패킷이 전달되지 않도록 차단(Drop)

#### Counters

- FlowTable에 제어 정보가 등록된 순간부터 현재까지의 시간을 측정하는 용도
- FlowTable에 등록된 제어 정보는 영구적으로 저장하거나 정해진 시간 동안만 유지
- Counters는 LifeCycle 관리에 사용