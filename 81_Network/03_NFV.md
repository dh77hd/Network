# # NFV

## 1. 개념

### 1-1. NFV란

- Network Function Virtualization
- 기존 NW 하드웨어로 구현되었던 라우터, LB, FW, IPS 등을 소프트웨어 형태의 가상 Appliance로 구현한 것

## 2. 구성

### 2-1. NFVO

- 네트워크 서비스 라이프사이클 관리
- NSD 템플릿, NS 인스턴스 관리
- VNFM, VIM 제어

### 2-2. VNFM

- VNF 라이프사이클 관리
- VNF 인스턴스 생성, 확장, 업데이트, 종료
- 기존 네트워크 기능들을 카탈로그화
- 전통적인 관리기능인 FCAPS 기능 필요 (Fault / Configuration / Accounting / Performance / Security)

### 2-3. VIM

- NFV 영역이 아닌 클라우드 영역
- 네트워크 기능 가상화를 위한 가상 인프라(NFVI) 관리
- 컴퓨팅, 네트워크, 스토리지 자원 등을 통합 관리
- 상위 인터페이스로는 가상 자원 관리, 하위 인터페이스로는 하드웨어나 네트워크 컨트롤러 같은 다양한 인터페이스 지원

