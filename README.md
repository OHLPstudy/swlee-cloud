# 말하고자 하는 게 뭔가요?
 - 실제 운영되고 있는 클라우드 서비스 환경이 어떤 케이스로 구축되어 있는지 알고 싶어요
 - 말하려고 하시는 Private Subnet을 만들어 둔다면 Public Subnet을 이용한 환경과 어떠한 차이점이 있고 장점이 있나요?
# 목차
 1. VPC
 2. Subnet
 3. NAT
 4. ELB ( OR ALB)
 5. Pulbic Subnet
 6. Private Subnet
 7. TLS HandShake
 8. Private Subent 장점

## VPC (Virtual Private Cloud) 
 - 쉽게 말해 가상 사설망
### e.g)
<img width="720" height="455" alt="image" src="https://github.com/user-attachments/assets/eb0ffc42-4cac-4d2f-95e6-adfab72cdbd6" />

해당 이미지는 실제 모 회사의 물리적인 내부 네트워크 망입니다. 
해당 회사는 후에 사무실A와 사무실B의 네트워크망을 분리해야하는 상황이 생겼습니다. 전통적인 물리적 네트워크 망이라면 해당하는 하드웨어 구조를 바꾸거나 
최악인 경우 인터넷 선공사 또한 다시 해야합니다.
<img width="720" height="521" alt="image" src="https://github.com/user-attachments/assets/3e12cd4e-4a15-493e-b3af-b14b6850ef5f" />

가상 사설망인 VPC를 이용한다면 네트워크 A, 네트워크 B가 같은 네트워크 망에 있지만 다른 네트워크인 것처럼 동작합니다. 이를 우리는 '가상 사설망이라고 부릅니다'

<img width="720" height="575" alt="image" src="https://github.com/user-attachments/assets/9ab08214-c57c-4259-add6-8aebcb192c79" />

만약 AWS 클라우드 환경에 VPC가 적용된다면 VPC별로 네트워크를 구성할 수 있고 각각 VPC에 따라 다르게 네트워크 설정을 줄 수 있습니다.
한마디로 독립적 네트워크 환경 구축이 가능합니다.

## Subnet
IP 주소를 나누어 리로스가 배치되는 물리적인 주소범위를 뜻한다



