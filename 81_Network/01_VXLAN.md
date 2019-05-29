# # VXLAN

## 1. Network Overlay  

- 물리 네트워크 위에 성립되는 가상 네트워크
- MAC over IP/UDP 기술 기반
- MAC 트래픽을 논리적인 터널을 통해 캡슐화하여 전달하는 것



## 2. VXLAN

### 2-1. Why?

- 가상화 환경에서  MAC address table, VLAN 숫자 한계
- 고정적인 VLAN Trunk 환경에서  VM 이동성 한계



### 2-2. What?

- L3 네트워크 위에 가상의 L2 네트워크를 오버레이하는 기술
- 오버레이된 네트워크들은 VXLAN  segment라고 하며, 각각의 segment에 포함된 VM끼리만 통신 가능
- 24비트의 식별자(VNI) 사용하여 캡슐화 = 터널링
- 터널의 종단(VXLAN Tunnel End Point = VTEP)은 VM을 서비스하고 있는 서버, 하이퍼바이저에 위치(물리적인 네트워크 장비도 가능)

 

### 2-3. VXLAN 터널 구성

1. VXLAN이 적용된 VM은 VXLAN을 인지 못하며, 다른 VM과 통신하기 위해 일반적인 MAC 프레임 전송
2. 물리 호스트 위의 VTEP은 프레임을 수신하고, 해단 VM 이 속한 VNI 를 찾음. 목적지 MAC이 같은 세그먼트에 속하는 VM이라면, 원격 VTEP의 MAC 매핑
3. L3 네트워크를 타고 전송된 프레임이 원격 VTEP에 도착. 원격 VTEP은 VNI를 검증하고 목적지 VM이 해당 VNI에 속해 있는지 확인한 뒤, 프레임의 캡슐화를 풀고 VM에 전달


