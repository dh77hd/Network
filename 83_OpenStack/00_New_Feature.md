- OVN : Open Virtual Network. 인스턴스들에 네트워크 서비스를 제공해주기 위한 OVS 기반의 네트워크 가상화 솔루션.
- SR-IOV : Single Root Input/Output Virtualization and Sharing. 인터페이스 단에서 가상화 지원. 커널이나 하이퍼바이저에서 NIC을 가상화하는  부담 제거. 

![](https://github.com/dh77hd/Note/blob/master/83_OpenStack/image/1.PNG?raw=true)

- DPDK : Data Plane Development Kit. 인텔에서 만든 라이브러리와 NIC 드라이버 모음. 



# Queen(Ver.13) Update

## # New Feature

1. flavor(nova) 에 vGPU 자원 할당 지원
2. 여러 VM이 1개의 Cinder 볼륨 사용
3. Ironic로 관리하는 베어메탈 서버에 대한 복구
   - Ironic Rescue Mode
   - 설정 잘못으로 부팅 불가능하거나 ssh 키를 잃어버려서 초기화/복구가 필요한 상황에 활용
4. k8s 네트워킹 정보를 k8s API 대신 Kuryr CNI Daemone에서 제공
5. GUI 기반의 Drag&Drop으로 Heat Template 생성
   - 드래그앤드랍으로 바로 배포되는 것이 아닌, Heat Template 파일 내용 자체를 구성하는 거라 Heat 문법 내용을 알아야 사양 가능
   - AWS CloudFormation과 유사
   - AWS CF도 AWS 초보자들은 다루기 어려워서, 만들어진 AWS CF Template 파일을 복사해서 변수 변경해서 사용



## # New Project

### 1. Helm

> 오픈스택 서비스를 컨테이너 이미지화한 후, Helm을 사용하여 K8S 에서 라이프사이클 관리



### 2. Masakari

> Instance HA
>
> VM 장애 발생 시, 자동으로 다른 정상 컴퓨트 노드에 해당 VM이 복구



### 3. LOCI

> OCI(Open Container Initiative) images of OpenStack services for containerization
>
> OCI와 호환되는 OpenStack 서비스 이미지를 만드는 프로젝트



### 4. Cyborg

> 다양한 가속 관련 인터페이스(암호화, 네트워크 가속화 등)에 대한 관리 프레임워크 제공
>
> OpenStack acceleration as a Service