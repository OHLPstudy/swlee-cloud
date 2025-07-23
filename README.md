# 말하고자 하는 게 뭔가요?
 - 실제 운영되고 있는 클라우드 서비스 환경이 어떤 케이스로 구축되어 있는지 알고 싶어요
 - 말하려고 하시는 Private Subnet을 만들어 둔다면 Public Subnet을 이용한 환경과 어떠한 차이점이 있고 장점이 있나요?
# 목차
 1. VPC
 2. Subnet
 3. NAT
 4. AWS ELB 
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
IP 주소를 나누어 리소스가 배치되는 물리적인 주소범위를 뜻한다 
VPC가 논리적인 범위를 말한다면 서브넷은 VPC안에서 실제로 리소스가 생성될 수 있는 네트워크 영역

## NAT (Network Address Translation)
https://aws-hyoh.tistory.com/145
IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고받는 기술

<img width="1710" height="827" alt="image" src="https://github.com/user-attachments/assets/4126e9ac-5bef-4037-af34-1bf163817707" />

사용자 1(10.10.10.10/24)이 공유기를 통해 공인망에 존재하는 웹 서버(125.209.22.142:80)에 접속하려고 합니다. 사용자 1은 사설 IP를 보유하고 있기 때문에 공인망으로 나아가기 위해서는 자신의 사설 IP를 공인 IP로 반드시 변환(NAT)해야 합니다. 그리고 이를 NAT Device(이하 NAT 장비, 공유기 등)가 수행하지요.

<img width="1109" height="454" alt="image" src="https://github.com/user-attachments/assets/8a093bae-f3c1-49d2-ae8b-c28beef8d5b1" />

 사용자의 사설 IP 인 10.10.10.10 은 NAT 디바이스에서 기억하고 있습니다.

<img width="1280" height="462" alt="image" src="https://github.com/user-attachments/assets/38d39ad1-bf66-4eef-8957-c8dc9aed8637" />

NAT 장비에 할당 된 공인 IP가 하나이지만 사용자가 두명일 경우 출발지 Port를 다르게 하여 사용자를 구분함

## AWS ELB (Elastic Load balancer)
https://aws-hyoh.tistory.com/128

<img width="706" height="600" alt="image" src="https://github.com/user-attachments/assets/962db0d0-38b6-4875-8edc-10cbaf7db36f" />

(여기서는 예를 EC2 인스턴스로 두었지만, 컴퓨팅 서비스면 해당 아키텍처가 될 수 있음)
AWS의 로드밸런서 서비스, 서버 부하 분산 즉 로드 밸런싱으로 특정 인스턴스(컴퓨팅)에 부하가 몰리지 않도록 적절히 분산 하는 것입니다.
부하 분산 대상에 대한 헬스 체크를 통해 다운 서버 제외 등이 가능합니다.

<img width="2037" height="823" alt="image" src="https://github.com/user-attachments/assets/d6a1cf31-d741-42bf-9bff-a4b60be1dade" />

### Application Load Balancer(ALB)
OSI 7 Layer의 7계층에 해당하는 로드밸런서
HTTp, HTTPS 특성을 주로 다루는 로드밸런서로 단순 부하뿐만 아니라 HTTP의 헤더 정보를 이용해 부하분산을 하는것이 ALB의 가장 중요특징
또한 SSL 인증서를 탑재 할 수 있어서 EC2를 대신하여 SSL 암복호화를 대신 진행할 수 있음

### Network Load Balancer(NLB)
OSI 7 Layer의 4계층에 해당하는 전송게층의 특성을 이용하는 로드밸런서
즉 TCP와 UDP를 사용하는 요청을 받아들여 부하분산 실시 HTTP도 TCP 기반 프로토콜이므로 NLB 사용이 가능하나 
하위 레이어인 TCP Layer에서 처리하므로 Http Header 해석불가

## Public Subnet
https://dontbesatisfied.tistory.com/13
외부에서 접근 가능한 네트워크 영역
구체적으로는 서브넷이 인터넷 게이트 웨이(igw) 로 향하는 라우팅 테이블과 연결되는 경우 퍼블릭 서브넷(당연하죠?)
해당 서브넷은 인터넷과 연결이 가능하므로 해당 서브넷에 위치한 리소스들은 공인 IP를 가질 수 있습니다.

## Private Subnet
외부에서 접근이 불가능한 네트워크 영역
인터넷 게이트웨이로 향하는 라우팅 테이블과 연결되어 있지 않으므로 외부 연결이 불가
해당 서브넷은 인터넷과 연결이 불가능하므로 해당 서브넷에 위치한 리소스들은 공인 IP를 가질 수 없습니다.
단 NAT 게이트웨이를 이용하여 내부에서 외부로만 접근이 가능하게 할 수 있습니다.

## Tls Handshake
https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/
<img width="2918" height="1667" alt="image" src="https://github.com/user-attachments/assets/373ba0ec-398a-41fb-a0f0-38b7836e2b04" />

TLS는 안전한 인터넷 통신을 위한 암호화 및 인증 프로토콜입니다. TLS 핸드셰이크는 TLS 암호화를 사용하는 통신 세션을 실행하는 프로세스입니다. 
TLS 핸드셰이크 중에, 통신하는 양측에서는 메시지를 교환하여 서로를 인식하고 서로를 검증하며 사용할 암호화 알고리즘을 구성하고 세션 키에 합의합니다. TLS 핸드셰이크는 HTTPS 작동 원리의 근간을 이룹니다.
해당 핸드쉐이크는 HTTPS 통신이 발생할때마다 매번 TLS 핸드셰이크가 발생

### 그러면 HTTP 통신은 TLS 핸드쉐이크를 하지 않습니까?
네

### 하지만 TCP 핸드셰이크는 합니다.
https://doozi0316.tistory.com/entry/HTTPHTTPS%EB%9E%80-TCP-UDP-HandShake-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC
HTTP 통신또한 TCP 레이어에 속하므로 당연히 3(4) way HandShake를 합니다.
이 과정은 목적지의 서버가 요청을 받을 수 있는 상태인지 확인하는 행동이므로 목적성이 다릅니다.
그러므로 HTTPS는 TCP HandShake, tls HandShake 두 단계를 모두 진행합니다.

## Private Subnet 장점
 - 외부 접근 차단
 - VPC 내부 통신

### Private Subnet 를 이용한 아키텍처를 굳이 고집할 필요가 있습니까?
외부에서 접근이 가능하다는 것은 보안적으로 취약점을 가질 수 밖에 없습니다. 
외부에서 접근할수 있는 포인트는 ELB로 두어 위험할 수 있는 어플리케이션 서버는 Private Subnet영역에 두는 것이 좋다고 생각합니다.
또한 ALB 자체에서 SSL 통신을 진행하고 내부 인스턴스에서는 HTTP 통신으로 진행된다면 TLS 핸드쉐이크를 진행 할 필요가 없어 인스턴스에 부하가 줄어듭니다.

### Public Subnet에서도 같은 VPC 영역에 있다면 Private Ip로 충분히 HTTP 통신을 진행 할 수 있는데요?
맞음
외부 접근은 SSL 이 적용 된 도메인으로 통신(Pulbic IP)받고 인스턴스끼리는 Private IP로 통신하여 HTTP 통신을 진행 할 수 있습니다.
하지만 보안적으로 취약점이 생기고, 굳이 그럴 필요가 없습니다.

### SSL Termination
https://blog.omoknooni.me/133
앞서 말한 ALB 같은 로드밸런서에 SSL 관리포인트를 주는 방법을 SSL Termination 이라고 합니다.

<img width="1280" height="646" alt="image" src="https://github.com/user-attachments/assets/cdfbc43f-b9f3-456c-9acd-f3a8f681c8b2" />

하지만 내부 백엔드 서버단에서 SSL 암호과 풀린 채로 전달되면 MITM(중간자공격)  (두 당사자 통신 가로채어 조작하는 사이버공격임)
에 노출되어 보안적으로 취약해질수 있습니다.
하지만 Private Subnet 영역에서 내부 통신이 이뤄진다면 MITM에서 어느정도 자유로울 수 있습니다.
어느정도라고 한 이유는 내부에서 뚫리거나 AWS 내부 네트워크의 취약점으로 네트워크 레이어가 뚫리면 문제가 될 수 있음
