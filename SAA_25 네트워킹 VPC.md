### CIDR, 비공개 및 공개 IP
- CIDR(사이더): 클래스 없는 도메인 간 라우팅
- IP 주소를 할당하는 방법으로 보안 그룹 규칙과 AWS의 네트워킹을 다룰 때 사용
- CIDR는 단순한 IP 범위를 정의하는 데 사용
	- /32로 끝나는 IP 주소는 IP 하나만 의미
	- 0.0.0.0/0이라면 모든 IP
	- 192.168.0.0/26는 -> 192.168.0.0~192.168.0.63까지 IP 주소 64개
- CIDR 구성 요소
	- 기본 IP란 범위에 포함된 IP
		- 예시로 10.0.0.0 또는 192.168.0.0 등
	- 서브넷 마스크
		- IP에서 변경 가능한 비트의 개수를 정의
		- /0, /24 그리고 /32까지 있음
			+ /8은 서브넷 마스크 255.0.0.0과 동일
			+ /16은 255.255.0.0과 동일
- IP 주소/32는 2의 0승으로 IP 하나만 허용
	- 192.168.0.0
- IP 주소/31이라면 두 가지 IP가 허용됨, 2의 1승으로 2전까지
	- 192.168.0.0과 192.168.0.1 
- 동일한 IP 주소/30의 경우 기하급수적으로 증가하는데 IP 네 개가 허용되어 .0부터 .3까지 범위가 더 넓어짐
	- 2의 2승인 4 전까지 0~3을 사용
- /29는 IP 여덟 개를 허용하고 범위는 .0부터 .7
- /28은 .0부터 .15까지 IP 16개를 허용
	- 192.168.0.0 ~ 192.168.0.15
- /24은 256개, 즉 2의 8승 개 IP가 있고, 범위는 .0부터 .255까지
	- 192.168.0.0 ~ 192.168.0.255
- /16은 2의 16승 개 IP가 바뀌니까 65,536개가 됨 -> IP 마지막 두 자리가 바뀜
	- 192.168.0.0 ~ 192.168.255.255
- /0은 모든 IP를 허용
- IP는 옥텟 네 개로 구성됨
	- /32는 옥텟 변경 불가
	- /24는 마지막 옥텟 변경 가능
	- /16은 마지막 옥텟 두 개가 변경되고 다른 값을 가짐
	- /8은 마지막 옥텟 세 개가 바뀜
	- /0은 옥텟 전체가 변경됨
- 192.168.0.0/24는 무슨 의미일까?
	- /24는 마지막 옥텟을 변경할 수 있으므로 IP가 256개라는 뜻
	- 마지막 옥텟이 0부터 시작해서 255까지를 포함해서
-  192.168.0.0/16은?
	- /16은 옥텟 마지막 두 개가 변경됨
	- 즉 IP 65,536개가 있음(2의 16승)
- 134.56.78.123/32 은?
	- /32는 IP가 한 개
- 0.0.0.0/0은 전체 IPv4 공간
- 공용 IP와 사설 IP 차이는?
	- Internet Assigned Numbers Authority 즉 IANA에서 구축한 특정 IPv4 주소 블록은 사설 LAN 네트워크(로컬 네트워크)나 공용 인터넷 주소임
	- 사설 IP는 특정 값만 허용
		- 주소가 10.0.0.0/8일 때 범위는 10.0.0.0 ~ 10.255.255.255로 아주 많아 대형 네트워크에서 사용
		-  172.16.0.0/12는 또 다른 사설 IP 주소 세트
			- 계정을 생성할 때 AWS에서 제공하는 기본 VPC는 해당 네트워킹 공간에 포함됨
		- 192.168.0.0/16은 홈 네트워크
- 장치를 연결할 때 인터넷 라우터가 있으면 192로 시작하는 IP 주소를 흔히 사용
### 기본 VPC 개요
- 새로운 AWS 계정은 모두 기본 VPC가 있고, 바로 사용 가능
- 새로운 EC2 인스턴스는 서브넷을 지정하지 않으면 기본 VPC에 실행됨
- 계정을 시작하면 VPC는 하나만 생김
- <u>기본 VPC는 처음부터 인터넷에 연결돼 있어서</u> 인스턴스가 인터넷에 액세스하고 또 내부의 EC2 인스턴스는 공용 IPv4 주소를 얻습니다 EC2 인스턴스를 생성하자마자 연결할 수 있음
- EC2 인스턴스를 위한 공용 및 사설 IPv4 DNS 이름 있음
- VPC에서 IPv4 CIDR 블록을 확인할 수 있고, 범위 내 첫 번째 IP와 마지막 IP가 /16이면 IP의 마지막 옥텟 두 개가 변경될 수 있다는 뜻으로 .255.255까지 가능
	- 범위 내 IP는 65,536개
- CIDR에 IP 하나만 생성되고, VPC와 연결되고, IPv6 CIDR는 없고 플로우 로그는 비활성상태로 가정
- 각 서브넷에는 고유 IPv4 CIDR가 있고, 서브넷마다 다른 AZ에 위치
- 기본 서브넷에 생성된 EC2 인스턴스는 전부 공용 IPv4를 가짐
- 라우팅  테이블은 트래픽이 VPC를 통해 경로를 선택하도록 도움
	- 0.0.0.0/0의 타켓이 igw-~~ 로 설정되어 <u>CIDR 외부 트래픽 모두 인터넷 게이트웨이로 이동함</u>
	- 인터넷 게이트웨이가 VPC에 연결되어 있고, VPC에 위치한 EC2 인스턴스에 인터넷 액세스를 제공함 -> EC2 인스턴스가 인터넷 액세스를 가질 수 있게 됨
- 라우팅 테이블은 명시적으로 연결된 서브넷은 없지만 암시적으로 연결된 경우는 있음 -> 서브넷에 할당된 라우팅 테이블이 없을 때 기본 라우팅 테이블을 무조건 할당
	- 필요시 원하는 서브넷으로 명시적으로 변경해야 함
### VPC 개요
- Virtual Private Cloud를 줄여 VPC
- 단일 AWS 리전에 여러 VPC를 둘 수 있음
- 리전당 최대 5개까지 가능
- VPC마다 할당된 CIDR는 다섯 개
- 각 CIDR의 최소 크기는 /28로 IP 주소는 최소 16개가 있고, 최대 크기는 /16으로 IP 주소는 최대 65,536개
- VPC가 사설 리소스이기 때문에 사설 IPv4 범위만 허용됨
- VPC CIDR가 다른 VPC나 네트워크 혹은 기업 네트워크와 겹치지 않도록 주의
### 서브넷 개요
- 서브넷을 두 개 만든다고 가정 하나는 공용 서브넷이고, 다른 하나는 사설 서브넷
	+ 모두 한 가용 영역에 있음
- 서브넷이란 VPC 내부에 있는 IPv4 주소의 부분 범위
- 이 범위 내에서 AWS가 IP 주소 다섯 개를 예약함
- IP 주소 처음 네 개, 마지막 한 개를 서브넷마다 예약 
	+ 이 IP 주소는 사용도 안 되고, EC2 인스턴스에 IP로 할당되지도 못함
- 예를 들어 CIDR 블록 10.0.0.0/24가 있다면 일부는 예약된 IP 주소
	- 10.0.0.0.0 : 첫번째는 네트워크 주소
	- 10.0.0.1 : 두번째 .1을 VPC 라우터 용으로 예약
	- 10.0.0.2 : Amazon 제공 DNS에 매핑
	- 10.0.0.3 : 당장 사용하지 않지만 여분으로 예약
	- 10.0.0.255 : 예약은 되지만 네트워크 브로드캐스트 주소인 .255이기 때문에 사용하지 않음
- <u>EC2 인스턴스 서브넷에서 IP 주소 29개가 필요할 때 /27 서브넷은 사용 못 함!</u>
	+ /27 IP 주소는 32개인데 예약된 IP 주소 다섯 개를 제외하면 27개만 남기 때문
	- 29개가 필요하댔는데 그보다 작기 떄문에 서브넷 크기는 /26이어야 함
		- 서브넷에 IP 주소 64개를 제공하기 때문, 5개를 빼더라도 59개로 넉넉함
### 인터넷 Gateway 및 라우팅 테이블
- `인터넷 Gateway`는 VPC의 리소스를 인터넷에 연결하도록 허용하는 EC2 인스턴스나 람다 함수 등
- 수평으로 확장되고 가용성과 중복성이 높은 관리형 리소스
- VPC와는 별개로 생성해야 하고, <u>VPC는 인터넷 Gateway 하나에만 연결</u>되며 반대의 경우도 마찬가지
- 인터넷 Gateway 자체는 인터넷 액세스를 허용하지 않음 
	+ 라우팅 테이블 수정이 필요
	+ VPC 안에 하나의 AZ에 서브넷이 퍼블릭, 프라이빗 서브넷이 각각 존재한다고 가정
	+ VPC에 인터넷 게이트웨이를 생성 -> 인터넷을 제공하지 못함
	+ 퍼블릭 서브넷에 퍼블릭 EC2 인스턴스를 생성 후 라우팅 테이블을 퍼블릭 서브넷에 생성
	+ 인터넷 게이트웨이와 라우팅 테이블을 연결하면 그 사이에 라우터가 있고 인터넷이 연결됨
### Bastion 호스트
- 사용자가 private 서브넷 EC2 인스턴스에 액세스하고자 함
- 사용자는 public 인터넷에 있음 ->  사용자의 EC2 인스턴스가 private 서브넷에 위치하지 않기 때문
	- private 서브넷에 대한 인터넷 액세스는 없음
- bastion 호스트는 EC2 인스턴스인데 이 EC2 인스턴스는 `public subnet`에 있음
- bastion host 보안 그룹이라는 자체 보안 그룹이 필요
- private 서브넷에 있는 EC2 인스턴스에도 보안 그룹도 필요
- public 서브넷에 있는 EC2 인스턴스 즉, 배스천 호스트로 private 서브넷에 있는 EC2 인스턴스에 액세스할 수 있음 ->  VPC가 존재하기 때문
- private 서브넷에 있는 EC2 인스턴스에 액세스하려면 먼저 SSH를 배스천 호스트에 연결하고, 이 배스천 호스트가 다시 SSH를 private 서브넷의 EC2 인스턴스에 연결해야 함
- 배스천 호스트를 통해 프라이빗 EC2 인스턴스에 SSH로 액세스할 수 있음
	+  <u>베스천은 꼭 퍼블릭 서브넷에 있어야 함</u>
- 배스천 호스트를 위해서는 <u>배스천 호스트 보안 그룹이 반드시 인터넷 액세스를 허용</u>해야 함
- 하지만 모든 인터넷 액세스를 허용하면 보안상 위험이 크기 때문에 가령 기업의 퍼블릭 CIDR 액세스만 허용하거나 사용자의 인터넷 액세스만 허용하는 등 제한을 둠
	- 배스천 호스트의 EC2 보안 그룹을 최대한 제한하여 특정 IP만 액세스가 가능하도록 설정할 수 있다는 뜻
- 프라이빗 서브넷의 EC2 인스턴스 보안 그룹에서는 반드시 SSH 액세스를 허용해야 함
	- `포트 22`번이 배스천 호스트의 프라이빗 IP가 되거나 배스천 호스트의 보안 그룹 되는 것
- 트래픽, 즉 EC2 인스턴스가 배스천 호스트를 이용하여 연결되기 때문
### NAT 인스턴스
- 구형 NAT 인스턴스이고, NAT Gateway는 최신
- NAT는 네트워크 주소 변환을 뜻함
- NAT 인스턴스는 사설 서브넷 EC2 인스턴스가 인터넷에 연결되도록 허용함
- NAT 인스턴스는 public 서브넷에서 실행되어야 하고, 공용 및 사설 서브넷을 연결함
- EC2 setting 중 <u>Source/Destination 확인은 비활성화</u> 해야 함
- <u>NAT 인스턴스에는 고정된 탄력적 IP가 연결되어야 함</u>
	- 예를 들어 공용 서버이며 IP는 50.60.4.10인 서버가 있음. 이때 사설 서브넷에서 액세스하려면 공용 서브넷에 고유의 보안 그룹이 있는 NAT 인스턴스를 실행하면 됨
	- 다음은 NAT 인스턴스에 탄력적 IP를 연결
		+ ex) EIP: 12.34.56.78 
	- 라우팅 테이블을 수정하여 사설 서브넷과 공용 서브넷의 두 서브넷에 있는 EC2 인스턴스로부터 NAT 인스턴스로 트래픽을 전송하도록 함
	- 인스턴스의 IP는 공용 서버로 액세스하는데 NAT 인스턴스를 통과해야 함
	- 요청에는 소스 IP(private subnet에 존재하는 인스턴스 private IP)가 10.0.0.20 이고,  목적지(Dest)는 50.60.4.10이라고 가정
	- NAT 인스턴스는 해당 서버로 트래픽을 보냄
	- NAT 인스턴스를 걸치면 Dest: 50.60.4.10, Source IP: 12.34.56.78 로 변경되어 나감
	- 네트워크 패킷이 NAT 인스턴스로 재작성됨 
		+ NAT 인스턴스가 하는 일이 `네트워크 패킷의 재작성`, 그래서 소스 IP가 변경됨
	- 이제 목적지 서버에서 응답하는데 Source IP 자기 자신인 50.60.4.10 이고, Dest는 NAT 인스턴스의 공용 IP인 12.34.56.78 가 됨
	- NAT 인스턴스가 다시 EC2 인스턴스로 트래픽을 회신
- 소스는 공용 서버이고, 목적지는 사설 IP로 일부의 IP가 재작성되고, 또 NAT 인스턴스를 위해 소스 및 목적지 확인 설정을 EC2 인스턴스에서 비활성화해야 함
- NAT 인스턴스의 작동 방식을 크게 보면 공용 서브넷에 NAT 인스턴스를 생성하고, 거기에 탄력적 IP를 연결
- 그리고 라우팅 테이블을 통해 사설 인스턴스가 NAT 인스턴스에서 인터넷 Gateway까지 통신하도록 함
- NAT 인스턴스를 보면, 사전 구성된 Amazon Linux AMI를 사용할 수 있었지만 지원이 종료되어 NAT Gateway를 권장
- NAT 인스턴스는 가용성이 높지 않고, 초기화 설정으로 복원할 수 없어서 여러 AZ에 ASG를 생성해야 하고, 복원되는 사용자 데이터 스크립트가 필요하기에 복잡함 
- 작은 인스턴스는 큰 인스턴스에 비교해서 대역폭이 더 작음 또한 보안 그룹과 규칙을 관리해야 함
- 인바운드에서는, 사설 서브넷의 HTTP/HTTPS 트래픽을 허용하고, 홈 네트워크의 SSH도 허용함, 아웃바운드에서도 트래픽이 나가도록 함
### NAT Gateway
- NAT Gateway는 AWS의 관리형 NAT 인스턴스이며 높은 대역폭을 가지고 있음
- 가용성이 높으며 관리할 필요가 없음
- 사용량 및 NAT Gateway의 대역폭에 따라 청구됨
- NAT Gateway는 특정 AZ에서 생성되고 탄력적 IP를 이어받음
- EC2 인스턴스와 같은 서브넷에서 사용할 수 없어서 다른 서브넷에서 액세스할 때만 NAT Gateway가 도움 됨
- 공용 서브넷에서 NAT Gateway를 만들고, 사설 서브넷의 인스턴스와 연결
- 경로는 private subnet -> nat gw -> igw
	- 즉 **NAT Gateway에는 인터넷 게이트웨이가 있어야 함**
- 대역폭은 초당 5GB이며 자동으로 초당 45GB까지 확장할 수 있음
- 보안 그룹을 관리할 필요가 없음 -> 연결을 위해 어떤 포트를 활성화할 지 고려하지 않아도 됨
	- 사설 인스턴스와 서브넷이 있고 인터넷에 액세스가 없음
	- NAT Gateway를 public 서브넷에 배포함
	- NAT Gateway가 public 서브넷에 있으므로 public 서브넷은 이미 인터넷 게이트웨이에 연결됐고, NAT Gateway에 인터넷 연결이 생김
	- private 서브넷의 루트 테이블를 수정해 EC2 인스턴스를 NAT Gateway에 연결할 수 있음
- NAT Gateway의 고가용성은  단일 AZ에서 복원 가능하고, 단일 AZ 내에서만 중복되지만 AZ가 중지될 경우를 위해 다중 NAT Gateway를 여러 AZ에 두면 결함 허용(fault-tolerance)을 할 수 있음
	- 하나의 NAT Gateway가 하나의 특정 AZ에 있음
	- 두 번째 AZ에 두 번째로 NAT Gateway를 생성
	- 각 네트워크 트래픽은 AZ 안에 제한됨 
		- AZ가 중지되면 전체 AZ가 중지되는 것
	- 하지만 AZ-B에도 NAT Gateway가 있어서 잘 작동 됨
	- 라우팅 테이블을 통해 AZ를 서로 연결할 필요는 없음
	- 만약 AZ가 중지되면 그 AZ의 EC2 인스턴스가 액세스 불가 상태가 됨
- NAT Gateway는 특정 AZ에서 가용성이 높음
- AZ 전체에서 고가용성을 필요로 하는 경우 다른 AZ에 더 만들어야 합니다 반면 NAT 인스턴스는 인스턴스간 장애 조치 스크립트를 통해서 전체적인 관리를 해야 함
- 대역폭은 NAT Gateway마다 초당 최대 45GB
- 인스턴스에서는 사용하는 EC2 인스턴스에 따라 다른데 고급 인스턴스 유형일수록 더 많은 처리량을 가짐
- 유지 보수의 경우 NAT Gateway는 관리형 서비스이며, NAT 인스턴스는 사용자가 하고, OS 패치 등 소프트웨어가 필요
- 비용의 경우 시간당 비용과 NAT Gateway의 데이터 전송량을 더하고 반면 NAT 인스턴스는 시간당 EC2 인스턴스가 청구되며 EC2 인스턴스 유형과 크기에 따라 달라짐
- EC2 인스턴스를 통해 인터넷으로 나가는 네트워크 비용도 함께 청구됨
- NAT Gateway는 공용 IPv4와 사설 IPv4를 가지고 있으며 NAT 인스턴스도 같음
- NAT Gateway는 보안 그룹을 사용하지 않지만 NAT 인스턴스는  보안 그룹을 설정 필요
- NAT Gateway는 Bastion 호스트로 쓸 수 없고, NAT 인스턴스는 필요한 경우 Bastion 호스트로도 가능
### NACL(Network ACL) 및 보안 그룹
- 서브넷 보안 단계가 NACL임
- EC2로 요청이 들어오면 서브넷에 들어가기 전 먼저 NACL로 이동
- NACL에는 몇 가지의 인바운드 규칙이 정의되며 요청이 허용되지 않으면 내부로 들어갈 수 없음
	- NACL은 `무상태`	
- 첫 번째 요청이 NACL을 통과해서 서브넷 내부에 도달 후 보안 그룹 인바운드 규칙을 검토
- 만약 그 요청이 명시적으로 허용 받지 못하면 거부됨 
- 보안 그룹의 특징은 `상태 유지`
	- incoming request
		-  만약 요청이 보안 그룹의 인바운드 규칙을 통과해서 EC2 인스턴스에 도달하면 EC2 인스턴스는 그 응답으로 애플리케이션 관점에서 응답할 내용을 모두 회신
		- 아웃바운드 규칙은 자동으로 보안 그룹 수준에서 허용됨
			- 보안 그룹 특징인 상태 유지 때문에 -> 들어간 것은 전부 나올 수 있음
		- 보안 그룹 수준에서 아웃바운드 규칙이 항상 허용되는 이유가 상태 유지인데 NACL은 상태 없음 즉 무상태이기 때문에 NACL 아웃바운드의 규칙이 평가됨
		- 규칙이 통과하지 못하면 요청도 통과하지 못함
	- out request
		- 보안 그룹에서 EC2 인스턴스가 아웃바운드 요청을 함 -> 외부로 바인딩 된 요청을 하는 것
		- EC2 인스턴스는 먼저 www.google.com 등에 접속할 수 있는데 처음으로 평가될 규칙은 보안 그룹 아웃바운드 규칙
		- 트래픽이 EC2 인스턴스에서 웹으로 나가도록 허용할지 결정
		- 규칙이 적절하고 요청이 허가되면 요청은 또한 NACL 아웃바운드 규칙을 통과
		- 평가된 요청이 www.google.com에 도달하고, Amazon 웹 서비스로 다시 돌아오는 것
		- NACL이 무상태라서 NACL 인바운드 규칙이 평가될 수 있음
		- 마지막으로 보안 그룹 수준에서 서브넷의 인바운드 규칙은 무조건 허용됨
			- 보안 그룹 규칙의 상태 유지 특징 때문 
		- 따라서 예시의 보안 그룹 인바운드 규칙은 동일
			- 아웃바운드 규칙은 이미 허용되었고, 보안 그룹은 상태 유지이기 때문
- 네트워크 액세스 제어 목록인 NACL은 무엇일까?
- 서브넷을 오가는 트래픽을 제어하는 방화벽과 비슷함
- 서브넷마다 하나의 NACL이 있고, 새로운 서브넷에는 기본 NACL이 할당됨
- NACL 규칙 정의에서 규칙에는 숫자가 있고, 범위는 1부터 32,766까지
	- 숫자가 낮을수록 우선순위가 높아서 우선순위가 제일 높은 것은 1 가장 낮은 것은 32,766
	- 마지막 규칙은 \*\로 일치하는 규칙이 없으면 모든 요청을 거부
	- AWS는 규칙을 100씩 추가하는 것을 권장함
	- 새로 만들어진 NACL은 기본적으로 모두를 거부함
- NACL 사용 사례
	- 서브넷 수준에서 특정한 IP 주소를 차단하는데 적합
	- NACL은 서브넷 수준에 있으니 공용 서브넷 및 사설 서브넷 등에 위치
	- 기본 NACL은 시험에서 정말 중요한 부분
	- 기본 NACL는 연결된 서브넷을 가지고, 인바운드와 아웃바운드의 모든 요청을 허용하는 특수성을 가짐
	- IPv4를 지원하는 기본 NACL의 모습인데 모든 것이 나가고 들어올 수 있음
	- NACL이 모든 것을 드나들도록 허용하지 않으면 AWS를 시작할 때 심각한 디버깅을 해야 함
- Default NACL은 연결된 서브넷을 가지고 인바운드와 아웃바운드의 모든 요청을 허용하는 특수성을 가짐 -> Default NACL을 수정하지 않는 것을 추천
	- Default NACL이 기본적으로 서브넷과 연결되어 모든 것이 드나들도록 허용된다는 뜻
- 사용자 정의 네트워크 ACL이 필요하다면 만들 수 있음
- 임시 포트
	- 클라이언트와 서버가 연결되면 포트를 사용해야 함 -> IP 주소와 포트가 있음
	- 클라이언트는 규정된 포트의 서버에 연결함
		- HTTP 포트는 80 HTTPS 포트는 443 SSH 포트는 22 등
	- 클라이언트가 서버에서 회신 받을 때는 제외하고 서버가 서비스를 올릴 때 클라이언트는 규정된 포트에 접속
	- 서버도 응답을 하려면 클라이언트에 연결해야 함
	- 클라이언트는 기본적으로 개방된 포트가 없어서 클라이언트가 서버에 접속할 때 자체적으로 특정한 포트를 열게 되는데 이 포트는 임시라서 클라이언트와 서버 간 연결이 유지되는 동안만 열려 있음
	- OS에 따라 즉 운영 체제에 따라 포트 범위가 달라짐
		- Windows 10을 사용하면 49152에서 65535까지가 무작위 임시 포트로 선택된 포트 범위, Linux를 사용할 경우 범위는 32768에서 60999까지
- 클라이언트가 웹 서버에 연결되며 웹 서버에는 고정 IP 및 고정 포트가 있을 때 클라이언트에 11.22.33.44 IP가 있고, 한 번의 요청에 임시 포트 50105를 개방하려고 함
- 클라이언트에서 서버로 전송된 요청에는 목적지 IP 정보와 서버가 연결할 목적지 포트 요청과 관련된 페이로드가 있음
	- Dest IP: 55.66.77.88
	- Dest Port: 443
	- Payload
	- Src IP: 11.22.33.44 -> 소스 아이피는 응답을 위한 것
	- Src Port: 50105 -> 소스 포트는 응답을 위한 임시 포트
- 웹 서버가 다시 회신을 위해서 클라이언트와 연결될 때 아래 응답을 보냄
	- Src IP: 55.66.77.88 
	- Src Port: 443
	- Dest IP: 11.22.33.44
	- Payload
	- Dest Port: 50105 -> 포트가 필요할 경우 클라이언트가 전송했던 임시포트 재사용
		- 임시 포트라고 불리는 것은 연결 수명을 위해서만 할당되는 무작위 포트이기 때문
- 클라이언트가 데이터베이스에 연결된다고 가정할 때 사설 및 공용 서브넷이 있고, 각 서브넷에 연결된 NACL이 하나 있음 -> 웹 NACL과 DB NACL
- 클라이언트가 데이터베이스 인스턴스에 연결을 시작하면 허용되어야 하는 규칙은?
- 첫 번째 NACL은 포트 3306를 통해 TCP부터 데이터베이스의 서브넷 CIDR까지 아웃바운드를 허용해야 요청이 서브넷을 떠나지 않음
	- 웹 NACL은 Allow Outbound TCP
	- on port 3306
	- to DB Subnet CIDR
- 데이터베이스 측에서는 DB NACL가 웹 서브넷 CIDR에서 포트 3306으로 TCP를 인바운드함
	- DB NACL은 Allow Inbound TCP
	- on port 3306
	- from Web Subnet CIDR
- 데이터베이스가 클라이언트로 요청에 회신을 할 때?
- 클라이언트에게는 임시 포트가 있고, 요청에 대해 무작위 포트가 할당됨
- DB NACL은 포트 및 임시 포트에서 아웃바운드 TCP를 허용하는데 범위는 1,024에서 65,535
	- DB NACL은 Allow Outbound TCP
	- on port 1024-65535
	- to Web Subnet CIDR
- 웹 서브넷 CIDR로 아웃바운드 되면 웹 NACL은 DB 서브넷 CIDR의 임시 포트 범위에서 인바운드 TCP를 허용해야 함
	- 웹 NACL은 Allow inbound TCP
	- on prot 1024-65535
	- from DB Subnet CIDR
- NACL의 또 다른 특징은 다중 NACL 및 서브넷이 있다면 각 NACL 조합이 NACL 내에서 허용되어야 함 
	- CIDR 사용 시, 서브넷이 고유의 CIDR을 갖기 때문
- <u>NACL에 서브넷을 추가하면 NACL 규칙도 업데이트해서 연결 조합이 가능한지 확인해야 함</u>
- 보안 그룹과 NACL의 차이점?
	- 보안 그룹은 인스턴스 수준에서 작동하는데 NACL은 서브넷 수준에서 작동됨
	- 보안 그룹은 허용 규칙을 지원하지만 NACL은 허용과 거부 규칙을 모두 지원
	- NACL에서는 특정 IP 주소를 거부할 수 있음
	- 보안 그룹은 상태 유지이며 규칙과 무관하게 트래픽을 허용하고, NACL은 무상태이기 때문에 인바운드와 아웃바운드 규칙이 매번 평가되고, 임시 포트를 리마인더
	- 보안 그룹에서는 모든 규칙이 평가되고, 트래픽 허용 여부를 결정하지만 NACL에서는 가장 높은 우선순위를 가진 것이 먼저 평가됨 -> 첫번째 비교로 끝
	- 보안 그룹은 누군가 지정할 때 EC2 인스턴스에 적용되지만 NACL은 거기에 연결된 서브넷의 모든 EC2 인스턴스에 적용됨
### VPC Peering
- 다양한 리전과 계정에서 VPC를 생성할 수 있는데 AWS 네트워크를 통해 연결하고 싶을 때 사용 하는데 언제 필요할까?
- VPC가 모두 같은 네트워크에서 작동하도록 만들기 위해서 사용
- VPC는 다른 리전 및 계정 또는 같은 계정에도 있어서 이들을 연결하거나 연결하지 않고 싶다면 VPC 네트워크 CIDR가 서로 멀리 떨어져야 함
	- 연결했을 때 CIDR가 겹치면 통신을 할 수 없기 때문
- VPC 피어링은 두 VPC 간에 발생하며 전이되지 않음
- **서로 다른 VPC가 통신하려면 VPC 피어링을 활성화해야 함**
- 세 개의 VPC인 A, B, C가 있다고 가정, A와 B 사이에 피어링 연결을 만들 수 있고, 서로 연결됨. 그리고 B와 C 사이에​​다른 피어링 연결을 생성하면 서로 통신 가능. 하지만 A와 B, 그리고 B와 C가 연결되어 있더라도 A와 C의 VPC 피어링 연결을 활성화해야 그 둘이 통신을 할 수 있음
- VPC 피어링이 있을 때 활성화는 당연하고 VPC 서브넷 상의 루트 테이블도 업데이트해서 EC2 인스턴스가 서로 통신할 수 있게 함
- 계정에서 할 수 있지만 다른 계정 간에도 가능
	- 계정 A에서 계정 B로 VPC를 연결할 수 있음, 리전 간 연결도 가능
- 보안 그룹에서 다른 보안 그룹을 참조할 수 있었는데 동일 리전의 계정 간 VPC에서도 보안 그룹을 참조할 수도 있음
	- CIDR이나 IP를 소스로 가질 필요가 없고, 보안 그룹을 참조할 수도 있음
- VPC 피어링 연결을 추가해서 서로 다른 VPC를 연결
### VPC Endpoint
- AWS에서 DynamoDB와 같은 서비스를 이용한다고 하면 퍼블릭 액세스가 가능한데 NAT Gateway나 인터넷 게이트웨이 또는 인터넷 게이트웨이를 통해 DynamoDB에 액세스할 수 있지만 모든 트래픽은 퍼블릭 인터넷을 거쳐서 옴
- CloudWatch와 Amazon S3 등의 서비스를 이용할 때는 인터넷을 경유하지 않고, 프라이빗 액세스를 원할 수도 있음
- VPC 엔드포인트를 사용하면 퍼블릭 인터넷을 거치지 않고도 인스턴스에 액세스할 수 있음
- 프라이빗 AWS 네트워크만 거쳐서 바로 해당 서비스에 액세스할 수 있음
- NAT Gateway가 있는 퍼블릭 서브넷 EC2 인스턴스, 프라이빗 서브넷, 인터넷 게이트웨이가 있다고 가정, Amazon SNS 서비스에 세 가지 방법으로 액세스할 수 있음
- 프라이빗 서브넷과 그 안에 있는 EC2 인스턴스에서 Amazon SNS 서비스에 액세스한다고 할 때 먼저 EC2 인스턴스에서 NAT Gateway를 거쳐 인터넷 게이트웨이로 향하고 Amazon SNS 서비스에 퍼블릭으로 액세스 함
	- 퍼블릭 서브넷에 있는 EC2 인스턴스의 경우도 같이 인터넷 게이트웨이에서 바로 Amazon SNS 서비스로 향함 -> 비용이 많이 발생
	- NAT Gateway를 거칠 때 비용이 발생하고, 그 다음 인터넷 게이트웨이에서는 비용이 발생하지 않지만 허브가 여러 개 있어 효율적이지 않음
- Amazon SNS 서비스 예시
	- `VPC 엔드포인트`가 있다고 가정 VPC 엔드포인트는 VPC 내에 배포됨
	- 네트워킹을 구성해서 프라이빗 서브넷에 있는 EC2 인스턴스를 VPC 엔드포인트를 거쳐 직접 Amazon SNS 서비스에 연결할 수 있는데 이때 네트워크가 AWS 내에서만 이루어진다는 장점이 있음
	- 모든 AWS 서비스는 퍼블릭에 노출되어 있고 퍼블릭 URL을 갖는데 VPC 엔드포인트를 사용하면 AWS PrivateLink를 통해 프라이빗으로 액세스하므로 AWS에 있는 모든 서비스에 액세스할 때 <u>퍼블릭 인터넷을 거치지 않고도 프라이빗 네트워크를 사용할 수 있음</u>
	- VPC 엔드포인트는 중복과 수평 확장이 가능하고, 인터넷 게이트웨이나 NAT Gateway 없이도 AWS 서비스에 액세스할 수 있게 해주므로 네트워크 인프라를 상당히 간단하게 만들 수 있음
	- VPC에서 DNS 설정 해석이나 라우팅 테이블을 확인하면 됨
- 두 가지 VPC 엔드포인트 유형
	- Interface 엔드포인트
		- ENI를 프로비저닝하는데 ENI는 VPC의 프라이빗 IP 주소이자 AWS의 엔트리 포인트(entry point)
		- ENI가 있으므로 반드시 보안 그룹을 연결해야 함
		- 대부분의 AWS 서비스를 지원하고, 청구되는 요금은 시간 단위 또는 처리되는 데이터 GB 단위로 청구됨
		- 프라이빗 서브넷에 EC2 인스턴스가 있고, PrivateLink를 사용하는 인터페이스 유형 VPC 엔드포인트로 ENI를 통하여 이 서비스에 액세스할 수 있음 -> 모든 서비스에서 이용 가능
	- Gateway 엔드포인트
		- 게이트웨이를 프로비저닝하는데 이때 <u>게이트웨이는 반드시 라우팅 테이블의 대상이 되어야 함</u>
			- IP 주소를 사용하거나 보안 그룹을 사용하지 않고 라우팅 테이블의 대상이 되는 것
		- 게이트웨이 엔드포인트 대상으로는 <u>Amazon S3와 DynamoDB</u> 두 가지가 있음
		- 장점은 요금이 무료이고, 라우팅 테이블 액세스일 뿐이므로 자동으로 확장된다는 점
- 인터페이스 엔드포인트는 모든 서비스를 지원하고, 게이트웨이 엔드포인트가 Amazon S3와 DynamoDB만 지원한다면 Amazon S3나 DynamoDB에는 둘 중 어떤 엔드포인트를 써야 할까?
	- 게이트웨이 엔드포인트와 인터페이스 엔드포인트가 있는데 게이트웨이 엔드포인트를 선택하는 것이 유리 
		- 라우팅 테이블만 수정하면 되기 때문
		- 앱에서 Amazon S3에 무료로 액세스할 수도 있음
- 게이트웨이 엔드포인트는 비용이 들지 않고 확장성이 더 높지만 인터페이스 엔드포인트를 사용하면 비용이 발생
- 게이트웨이 엔드포인트보다 인터페이스 엔드포인트가 권장되는 경우는 <u>온프레미스에서 액세스</u>해야 할 필요가 있을 때
- 온프레미스에 있는 데이터 센터에 프라이빗으로 액세스해야 하는 경우 Site-to-Site VPN이나 직접 연결 방법이 있음
- 다른 VPC에 연결할 때도 인터페이스 엔드포인트 유형을 사용하는 편이 좋음
- 대부분 Amazon S3에서는 게이트웨이 엔드포인트가 유리
###  VPC Flow Logs
-  VPC Flow Logs를 사용하면 인터페이스로 들어오는 IP 트래픽에서 정보를 포착할 수 있음
	- VPC 수준
	- 서브넷 수준
	- 탄력적 네트워크 인터페이스 수준
- VPC에서 일어나는 연결 문제를 모니터링하고 해결하는 데 유용
- 흐름 로그를 Amazon S3, CloudWatch Logs, Kinesis Data Firehose에 전송할 수 있음
- ELB, RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway 등AWS의 관리형 인터페이스에도 전송할 수 있음
- 버전, 인터페이스 ID, 소스 주소, 대상 주소, 소스 포트, 대상 포트, 프로토콜, 패킷, 바이트 수, 시작, 끝, 액션, 로그 상태가 있음 
	- VPC로 가는 네트워크 패킷에 관한 메타데이터 
- 소스 주소(srcaddr)와 대상 주소(dstaddr)는 문제가 있는 IP를 식별하는 데 도움
	- 어떤 IP가 반복적으로 거부되는 걸 보면, 그 IP에 문제가 있을 수 있다는 점을 알 수 있음
	- 특정한 IP로부터 공격을 받고 있을 수도 있음
- 소스 포트(srcport)와 대상 포트(dstport)는 문제가 있는 포트를 식별하는데 유용
- 액션(action)은 수락 또는 거부가 되는데 보안 그룹 또는 NACL 수준에서 성공 혹은 실패 여부를 알려줌
- VPC Flow Logs를 이용해서 사용 패턴을 분석하거나 악의적인 행동이나 포트 스캔 등을 탐지할 수 있음
- Flow Logs를 쿼리하는 방법은 두 가지
	- 최적의 방법은 Athena를 S3에 사용하는 방법
	- 스트리밍 분석을 원한다면 CloudWatch Logs Insights를 사용할 수도 있음
- Flow Logs를 이용해서 어떻게 보안 그룹과 NACL이슈를 해결할 수 있을까?
	- ACTION 필드를 확인 -> NACL과 서브넷에 유입되는 요청을 확인할 수 있음
	- NACL은 상태가 없고, 보안 그룹에는 상태가 있음
	- Inbound REJECT가 되면 외부로부터 우리 EC2 인스턴스로 유입되는 요청이 거절되는데 NACL이 요청을 거부하거나 보안 그룹이 요청을 거부한다는 의미
	- Inbound ACCEPT, Outbound REJECT라면 NACL만의 이슈라는 뜻
		- 보안 그룹은 상태가 있기 때문에 허용하면 허용 거부하면 거부로 같은 결과
		- 요청에 대해서도 비슷한 분석이 가능
	- Outbound REJECT라면 NACL이나 보안 그룹에 이슈가 있다는 뜻
	- 하지만 Outbound ACCEPT이고 Inbound REJECT라면 반드시 NACL에 이슈가 있는 것
 - Flow Logs가 CloudWatch Logs로 갈 수 있고, CloudWatch Contributor Insights라는 걸 이용
	 - 예를 들어 네트워크에 가장 많이 기여하는 상위 10개 IP 주소를 확인할 수 있음
 - VPC Flow Logs를 사용해서 CloudWatch Logs로 전송할 수도 있음
	 - metric 필터를 설정해서 예를 들어 SSH 또는 RDP 프로토콜을 검색할 수 있음
	 - 평소보다 SSH 또는 RDP가 있다는 걸 알게 되면, CloudWatch Alarm을 트리거할 수 있음
 - Amazon SNS 토픽에 경보를 전송할 수도 있음
 - VPC Flow Logs를 사용하고 모든 걸 S3 버킷에 전송해서 저장할 수 있고, Amazon Athena를 사용해서 SQL로 된 VPC Flow Logs를 분석할 수 있고, Amazon QuickSight로 그걸 시각화할 수도 있음
### VPC Flow  기록 및 Athena
- 흐름 로그에 유형 -> DemoS3FlowLog
- 필터가 있는데 수락형, 거부형, 또는 모든 종류의 트래픽을 선택할 수 있음
- 어떤 트래픽이 통과되지 않는지 디버깅을 하려고 한다면 거부형이 더 적절하고 그렇지 않으면 All
- 최대 집계 주기에서는 정보를 확인하기 위해 집계를 얼마나 오래 기다려야 하는지를 지정
- 전송 대상을 CloudWatch Logs로 할 수 있고, 로그 그룹을 지정해야 함
- Amazon S3 버킷에도 전송할 수 있음
- S3 버킷 ARN을 지정하거나 동일한 계정에 있는 Kinesis Data Firehose에 전송할 수도 있음, 다른 계정에 있는 Firehose에도 전송할 수 있음
- Amazon S3 버킷을 생성하고, 버킷 ARN을 찾아 리소스 기반 정책이 자동으로 생성되고 타깃 버킷에 첨부되어 VPC 서비스는 데이터를 S3 버킷에 전송할 수 있음
- Create flow log로 로그 생성 가능
- 흐름 로그를 S3로 보내거나 CloudWatch Logs로 보낼 수 있음
- Athena를 사용해서 더 큰 분석을 할 수 있고,  Amazon S3를 사용
- Athena에서 Amazon S3의 쿼리 결과 위치를 설정해 저장할 S3 버킷을 선택할 수 있음
- 다른 구문을 실행
- Athena를 사용해서 Amazon S3에서  로그들을 쿼리하는 방법이 있음
### Site-to-Site VPN, 가상 사설 Gateway 및 고객 Gateway
- 특정 구조가 있는 기업 데이터 센터를 AWS와 비공개로 연결하려면 기업은 Customer Gateway를 VPC는 VPN Gateway 필요
- 공용 인터넷을 통해 사설 Site-to-Site VPN을 연결
	- VPN 연결이라서 암호화됨
- VPN은 가상 프라이빗 게이트웨이 즉 VGW가 필요
	- VPN 연결에서 AWS 측에 있는 VPN 집선기(concentrator)이며 Site-to-Site VPN 연결을 생성하려는 VPC와 연결됨
	- ASN을 지정할 수도 있음
- CGW는 고객 게이트웨이로 소프트웨어 혹은 물리적 장치로 VPN 연결에서 데이터 센터임
- 고객 게이트웨이가 있는 기업 데이터 센터와 가상 프라이빗 게이트웨이를 갖춘 VPC가 있을 때 온프레미스 고객 게이트웨이를 어떻게 구축해야 할까?
- 고객 게이트웨이가 공용이라면 인터넷 라우팅이 가능한 IP 주소가 고객 게이트웨이 장치에 있음, 이걸 사용해서 VGW와 CGW를 연결하면 됨 
	- 고객 게이트웨이의 공용 IP를 사용
- 고객 게이트웨이를 비공개로 남겨 사설 IP를 가질 수도 있는데 NAT 장치 뒤에 있는 NAT-T를 활성화
	- NAT 장치에 공용 IP가 있을 시 이 공용 IP를 CGW에 사용
- <u>서브넷의 VPC에서 라우트 전파(Route Propagation)를 활성화해야 Site-to-Site VPN 연결이 실제로 작동</u>
- 온프레미스에서 AWS로 EC2 인스턴스 상태를 진단할 때 보안 그룹 인바운드 `ICMP 프로토콜`이 활성화됐는지 확인해야 함 -> 그렇지 않음 연결 안 됨
- Site-to-Site VPN에서 `AWS VPN CloudHub`도 중요!
- VGW를 갖춘 VPC가 있고 고객 네트워크와 데이터 센터마다 고객 게이트웨이가 마련된 상황을 가정
	- CloudHub는 여러 VPN 연결을 통해 모든 사이트 간 안전한 소통을 보장
	- 비용이 적게 드는 허브 및 스포크 모델(hub&spoke)로 VPN만을 활용해 서로 다른 지역 사이 기본 및 보조 네트워크 연결성에 사용
	- VPC 내 CGW와 VGW 하나 사이에 Site-to-Site VPN을 생성
	- 고객 네트워크는 VPN 연결을 통해 서로 소통할 수 있음
	- VPN 연결이므로 모든 트래픽이 공용 인터넷을 통합해서 사설 네트워크로는 연결되지 않음
	- 공용 인터넷을 통하지만 VPN 연결은 당연히 암호화 됨
- VGW 하나에 Site-to-Site VPN 연결을 여러 개 만들어 동적 라우팅을 활성화하고 라우팅 테이블을 구성하면 됨
###  Direct Connect(DX) & 다이렉트 커넥트 게이트웨이
- Direct Connect(DX)는 원격 네트워크로부터 VPC로의 <u>dedicated private</u> 연결을 제공
- Direct Connect를 사용할 때는 dedicated connection을 생성해야 하고, AWS Direct Connect 로케이션을 사용
- VPC에는 가상 프라이빗 게이트웨이(VGW)를 설치해야 온프레미스 데이터 센터와 AWS 간 연결이 가능
- Amazon S3 등의 퍼블릭 리소스와 EC2 인스턴스 등의 프라이빗 리소스에 퍼블릭 및 프라이빗 VIF(virtual interface)를 사용해 액세스할 수 있음
- Direct Connect 사용 사례
	- 대역폭 처리량이 증가할 때 큰 데이터 세트를 처리할 때 속도가 빨라짐
		- 퍼블릭 인터넷을 거치지 않기 때문
	- 프라이빗 연결을 사용하므로 비용도 절약
	- 퍼블릭 인터넷 연결에 문제가 발생해도 Direct Connect를 사용하면 연결 상태를 유지할 수 있음 -> 프라이빗 연결이기 때문
	- 실시간 데이터 피드를 사용하는 애플리케이션에 유용
- 하이브리드 환경을 지원
	- 온프레미스 데이터 센터와 클라우드가 연결되어 있기 때문
- IPv4와 IPv6 둘 다 지원
- 리전을 기업 데이터 센터에 연결하려면 AWS Direct Connect Location을 요청
	- AWS Direct Connect Location에는 Direct Connect 엔드포인트가 cage 하나, 고객 혹은 파트너 router가 있는 Customer or Partner cage 총 2개가 있음
- 온프레미스 데이터 센터에는 방화벽이 있는 고객 라우터를 설치
- 모든 로케이션을 연결하는 프라이빗 가상 인터페이스(private VIF)를 생성하여 가상 프라이빗 게이트웨이(VGW)에 연결해야 함
	- 가상 프라이빗 게이트웨이(VGW)는 VPC에 연결되어 있음
- 프라이빗 VIF를 통해서 EC2 인스턴스가 있는 프라이빗 서브넷에 액세스할 수 있음
- 수동으로 연결해야 하므로 설치만 기간이 오래 걸릴 수 있음
- 퍼블릭 인터넷을 전혀 지나지 않고, 전부 비공개로 연결됨
- AWS 내 퍼블릭 서비스 Amazon Glacier 또는 Amazon S3에 연결할 수도 있는데 퍼블릭 가상 인터페이스 즉 public VIF를 설치해야 함
	- 가상 프라이빗 게이트웨이로 연결되지 않고, AWS로 직접 연결되는 것
- 다른 리전에 있는 하나 이상의 VPC와 연결하고 싶다면 `Direct Connect Gateway`를 써야 함
- 리전이 두 개인 예시가 있을 때 각기 다른 VPC와 CIDR가 두 개 있고, 온프레미스 데이터 센터를 양쪽 VPC에 연결한다고 가정
	- Direct Connect 연결을 생성한 뒤 프라이빗 VIF를 사용해 Direct Connect Gateway에 연결
	- 게이트웨이는 프라이빗 가상 인터페이스를 통해 가상 프라이빗 게이트웨이에 연결되어 있고, 첫 번째와 두 번째 리전 모두 같은 구성으로 되어 있음
	- 설정을 통해 여러 VPC와 여러 리전을 연결할 수 있음
- Direct Connect 유형
	- dedicated Connections(전용 연결)는 초당 1Gbp 혹은 10Gbp이 한도
		- 고객은 물리적 전용 이더넷 포트를 할당받음
		- 먼저 AWS에 요청을 보내면 AWS Direct Connect 파트너가 처리를 완료
	- Hosted Connections(호스팅 연결)도 가능한데 초당 50Mbp, 500Mbp에서 10Gbp까지 다양
		- AWS Direct Connect 파트너를 통해 또다시 연결을 요청
		- 전용 연결보다는 유연해 용량을 추가하거나 제거할 수 있음
		- 선택한 로케이션에서 초당 1, 2, 5, 10Gbp 이용이 가능하고,
	- 전용 연결이든 호스팅 연결이든새 연결을 만들려면 리드 타임이 한 달보다 길어질 때도 있음
		- 예를 들어 일주일 안에 <u>빠르게 데이터를 전송하고 싶다면 Direct Connect는 답이 될 수 없음</u> -> 설치 기간이 한 달 정도 걸리기 때문
		- Direct Connect가 이미 설치됐는지 파악하고, 데이터 전송 기간이 한 달보다 긴지 짧은지 확인해야 함
- Direct Connect에는 암호화 기능이 없어서 데이터가 암호화되지 않지만 프라이빗 연결이므로 보안을 유지할 수 있음
	- 암호화를 원한다면 Direct Connect과 함께 VPN을 설치해서 IPsec으로 암호화된 프라이빗 연결이 가능
	- 추가로 보안을 확보하면 좋지만 구현이 복잡하다는 단점
	- 암호화 과정은 Direct Connect 로케이션을 가져와 해당 연결에 VPN 연결을 구축하면 Direct Connect에 암호화가 구성
	- 기업 데이터 센터와 AWS 간 모든 트래픽을 암호화할 수 있음
- `복원력(resiliency)` 및 아키텍처 모드 두 가지 모두 중요!
- 핵심 워크로드의 복원력을 높이기 위해서는 여러 Direct Connect를 설치하는 것이 좋음
	- 기업 데이터 센터가 두 개 있고, Direct Connect 로케이션도 둘이라고 했을 때 중복이 발생 
	- 프라이빗 VIF가 하나 있는데 다른 데 또 있다면 하나의 연결을 여러 로케이션에 수립한 것이므로 Direct Connect 하나가 망가져도 다른 하나가 예비로 남아 있기 때문에 복원력이 강해짐 
	- 핵심 워크로드에 적합
- 핵심 워크로드 복원력을 최대로 끌어 올리고 싶다면 각 Direct Connect Location에 독립적인 연결을 두 개씩 수립하면 복원력을 최대로 만들 수 있음
	- 두 로케이션에 2개씩 DX를 연결하면  총 네 개의 연결을 수립해서 AWS에 연결
- 최대의 복원력을 달성하려면 여러 독립적인 연결을 하나 이상의 로케이션에서 각기 다른 장치에 도달하도록 구성하면 됨
### 다이렉트 커넥트 & Site to Site VPN
- 회사 데이터 센터가 Direct Connect를 통해 VPC에 연결한다고 가정
- 비용도 많이 들고 가끔은 Direct Connect 연결에 문제가 발생할 수도 있음
- VPC에서 인터넷 연결이 없는 경우는 피하고 싶음, 또 다른 Direct Connect로 보조 연결을 할 수 있지만 비용이 상당히 많이 듦
- Site to Site VPN을 백업 연결을 두어 기본 연결에 문제가 발생했을 때 이 연결을 사용하면 Site to Site VPN을 통해 <u>퍼블릭 인터넷에 연결할 수 있으므로 안정성이 높아짐</u>
- 퍼블릭 인터넷에 언제나 연결되어 있기 때문
### Transit Gateway
- AWS 네트워크 토폴로지는 VPC 여러 개를 피어링으로 전부 연결하고 VPN 연결과 Direct Connect를 구축해서 Direct Connect Gateway로 여러 VPC를 한 번에 연결하는 식으로 복잡함
	- `Transit Gateway`로 복잡함을 해결할 수 있음
- 전이적 피어링(transitive peering) 연결이 VPC 수천 개와 온프레미스 데이터 센터 Site-to-Site VPN, Direct Connect, 허브와 스포크 간 스타형 연결 사이에 생성됨
- Transit Gateway를 통해 VPC 여러 개를 연결할 수 있음
	- VPC를 모두 피어링할 필요가 없고 Transit Gateway를 통해 전이적으로 연결됨
- 모든 VPC가 서로 통신할 수 있는데 Direct Connect Gateway를 Transit Gateway에 연결하면 Direct Connect 연결이 각기 다른 VPC에 직접 연결
- Site-to-Site VPN과 VPN 연결을 선호한다면 고객 게이트웨이와 VPN 연결을 Transit Gateway에 연결할 수 있는데 <u>Transit Gateway 일부로 모든 VPC에 액세스하는 것</u>
	- 네트워크 문제 해결에 유용
- 리전 리소스이며 리전 간에 작동되어 Transit Gateway를 계정 간에 공유하려면 Resource Access Manager를 사용하고, 리전 간 Transit Gateway를 피어링할 수도  있음
	- Transit Gateway에 라우팅 테이블을 생성해서 어느 VPC가 누구와 통신할지 어떤 연결이 액세스할지 제한
	- Transit Gateway 내 모든 트래픽 경로를 제어해서 네트워크 보안을 제공
	- Direct Connect Gateway 및 VPN 연결과 함께 작동하고 AWS에서 유일하게 IP 멀티캐스트를 지원하는 서비스로 <u>IP 멀티캐스트가 나오면 Transit Gateway를 사용해야 함</u>
- Site-to-Site VPN 연결 대역폭을 ECMP(Equal-cost multi-path)를 사용해 늘리는 경우에도 사용
	- 등가 다중 경로 라우팅을 뜻하는 ECMP는 여러 최적 경로를 통해 패킷을 전달하는 라우팅 전략
	- Site-to-Site VPN을 이용해서  Site-to-Site VPN 연결을 많이 생성해서 AWS로의 연결 대역폭을 늘릴 때 사용
- Transit Gateway에 VPC 네 개가 연결되어 있다고 가정 
	- 기업 데이터 센터는 Site-to-Site VPN으로 Transit Gateway에 연결
	- Site-to-Site VPN 연결을 구축할 때 각각 앞쪽과 뒤쪽을 향하는 터널 두 개가 있고 Site-to-Site VPN을 VPC에 직접 연결하면 두 터널 모두 연결 하나로 사용되지만 Transit Gateway를 사용할 때는 두 터널이 동시에 사용
	- Transit Gateway로 여러 Site-to-Site VPN을 만드는데 두 번째 Site-to-Site VPN을 생성해 Transit Gateway로 연결할 수 있기 때문에 총 터널 네 개가 생김
	- Site-to-Site VPN에 터널 네 개가 있으면 연결 처리량이 증가
	- <u>기업 데이터 센터를 직접 VPC에 연결한 경우에는 사용할 수 없는 기능</u>
- 가상 프라이빗 게이트웨이에 VPN을 연결하면 VPC마다 터널 하나 즉 연결이 하나씩 생기고 연결 하나당 1.25Gbps를 제공하며 최대 처리량으로 제한 -> VPN 연결은 터널 두 개로 구성
- VPN을 Transit Gateway로 연결하면 Site-to-Site VPN 하나가 여러 VPC에 생성
	- 동일한 Transit Gateway에 모두 전이적으로 연결되기 때문
	- Site-to-Site VPN 연결 하나는 ECMP 덕에 최대 처리량이 2.5Gbps가 됨
- Transit Gateway에 Site-to-Site VPN 연결을 더 추가할 있음 
	- 두세 개 정도만 더해도 ECMP를 사용하면 처리량이 두세 배
- 데이터가 Transit Gateway를 통과할 때 1GB마다 요금이 청구됩니다 성능 최적화 비용이 추가됨
- Direct Connect 연결을 여러 계정에서 공유할 때도 Transit Gateway를 사용
- 기업 데이터 센터와 Direct Connect 로케이션 간에 Direct Connect 연결을 생성하면 됨
- Transit Gateway를 서로 다른 계정의 VPC 두 개 모두에 생성하고 Direct Connect 로케이션에 연결한 Direct Connect Gateway를 Transit Gateway에 연결하면 여러 계정과 VPC 사이에 Direct Connect 연결을 공유할 수 있음
### VPC 트래픽 미러링
- VPC 트래픽 미러링은 VPC에서 네트워크 트래픽을 수집하고 검사하되 방해되지 않는 방식으로 실행하는 기능
- 관리 중인 보안 어플라이언스로 트래픽을 라우팅하고 트래픽을 수집해 수집하려는 트래픽이 있는 소스 ENI를 정의
- 어디로 트래픽을 보낼지 대상을 정의 -> ENI나 네트워크 로드 밸런서에서
- EC2 인스턴스와 탄력적 네트워크 인터페이스가 있고, ENI가 연결되어 EC2 인스턴스는 인터넷에 액세스 중
	- ENI에서 EC2 인스턴스로 인바운드 및 아웃바운드 트래픽이 많이 생김
	- 트래픽을 분석하려면 로드 밸런서를 설정하는데 네트워크 로드 밸런서 뒤에 보안 소프트웨어가 있는 EC2 인스턴스의 오토 스케일링 그룹이 있음
	- 소스 A의 기능을 방해하지 않으면서 소스 A에서 발생하는 트래픽을 모두 수집하고 싶을 때 VPC 트래픽 미러링을 설정
	- 모든 정보가 아닌 일부만 얻고 싶다면 선택적으로 필터를 사용할 수도 있음
	- 트래픽 미러링 기능을 통해 ENI나 소스 A로 전송되는 트래픽은 전부 네트워크 로드 밸런서로도 보냄 -> 소스 A, EC2 인스턴스는 여전히 작동
	- 네트워크 로드 밸런서로 보내서 트래픽 자체를 분석 -> 트래픽 미러링
- 소스 하나가 아니라 여러 개에 적용되데 두 번째 EC2 인스턴스에 또 다른 ENI가 있다면 네트워크 로드 밸런서로 트래픽을 미러링할 수 있음
- VPC 피어링을 활성화한 경우 동일한 VPC에 소스와 대상이 있어야 하고, 다른 VPC에 걸쳐 있기도 함
- VPC 트래픽 미러링 사용 사례로는 콘텐츠 검사와 위협 모니터링, 네트워킹 문제 해결 등
### VPC용 IPv6
- IPv4의 대역이 모두 사용되어 IPv6가 만들어짐
- 3.4x10의 38승 개 고유 IP 주소를 제공
- 모든 IPv6 주소는 공용이고 인터넷 라우팅이 가능
- IPv6에는 사설 범위가 없고, 형식은 16진수 8자리로 나타냄
- 범위는 0000에서 ffff
- VPC에서 IPv6 지원을 활성화할 수 있음
- VPC와 서브넷에서 IPv4 비활성은 불가능하지만 IPv6를 활성화할 수 있습니다 공용 IP 주소이며 듀얼 스택 모드로 작동
- EC2 인스턴스가 VPC에서 실행되면 최소 사설 내부 IPv4 하나와 공용 IPv6 하나를 얻게 되고, IPv4나 IPv6를 사용해서 인터넷 게이트웨이를 통해 인터넷에 통신할 수 있음
- EC2 인스턴스에 사설 IP와 공용 IPv6가 있을 때 인터넷에 액세스하려면 인터넷 게이트웨이를 통해야 함
- 인터넷 게이트웨이는 IPv4와 IPv6에 연결성을 제공
- EC2 인스턴스는 공개적으로 액세스 가능한데 공용 IPv6 주소가 있고, 인터넷 게이트웨이가 인터넷 연결을 제공하기 때문
- IPv6 트러블 슈팅
- IPv4는 VPC 및 서브넷에서 비활성화될 수 없음
- 만약 IPv6가 활성화된 VPC가 있는데 서브넷에서 EC2 인스턴스를 실행할 수 없다고 하면 인스턴스가 IPv6를 받지 못해서는 아니고, 서브넷에 이용 가능한 IPv4가 없기 때문
	- 서브넷에 IPv4 CIDR를 생성해서 해결
- EC2 인스턴스를 계속 실행하면 사설 IPv4와 공용 IPv6를 얻게 됨
- IPv4 공간이 전부 소진된다면 사용자가 새 EC2 인스턴스를 생성할 시 오류가 발생
- 서브넷의 VPC에 새로운 CIDR를 추가해서 해결
### Egress-only Internet Gateway
- 송신 전용 인터넷 게이트웨이는 <u>IPv6 트래픽에만 사용</u>되고 IPv4인 NAT Gateway와 비슷하지만 IPv6 전용임
- VPC 인스턴스에서 IPv6상 아웃바운드 연결을 허용하고 동시에 인터넷이 인스턴스로 IPv6 연결을 시작하지 못하게 막는 기능 -> 라우팅 테이블 업데이트 필요
- VPC에 공용 서브넷이 있다면 인터넷 게이트웨이를 통해 EC2 인스턴스가 인터넷에 액세스하고, 인터넷은 IPv6를 사용하여 인스턴스로 연결을 시작할 수 있음
	- 인터넷 게이트웨이에 연결된 상태이고 공용 서브넷이기 때문
- 사설 서브넷의 경우 인터넷 게이트웨이가 없으니 서브넷이 비공개 상태로 송신 전용 인터넷 게이트웨이를 생성하면 사설 서브넷의 EC2 인스턴스가 송신 전용 인터넷 게이트웨이를 통해 IPv6로 인터넷에 액세스 -> 아웃바운드만 가능
- 하지만 인터넷에서 EC2 인스턴스까지 연결은 개시할 수 없음 -> 인바운드 불가
- IPv6가 있는 VPC가 있을 때 공용 및 사설 서브넷이 있는데 둘 다 IPv4와 IPv6를 가짐
- 인터넷에 액세스하는 웹 서버는 인터넷 게이트웨이를 통해 IPv4와 IPv6로 액세스
- 공용 서브넷의 라우팅 테이블에서 로컬 IPv4와 로컬 IPv6 트래픽이 있는데 서브넷 또는 VPC의 CIDR 내부에 있음
- 그 외에 0.0.0/0은 모든 IPv4를 뜻하고, ::/0은 모든 IPv6를 의미하는데 전부 인터넷 게이트웨이를 통해 웹 서버가 인터넷에 액세스하도록 허용
	- 공용 서브넷의 IPv6와 IPv4를 양방향으로 활성화하는 방법
- 사설 서브넷은 서버에 사설 IPv4와 IPv6가 있음
- 서버가 인터넷에 엑세스하되 액세스받지는 못하게 하려면 어떻게 해야 할까?
- IPv4의 경우 NAT Gateway를 사용
- 서버는 NAT Gateway에 연결되고, NAT Gateway는 인터넷 Gateway에 연결되며 IPv4로 인터넷에 액세스
- IPv6는 송신 전용 인터넷 게이트웨이를 사용해야 함
- 송신 전용 인터넷 게이트웨이에 서버가 연결되고, IPv6로 인터넷에 액세스
- 라우팅 테이블에서 0.0.0.0/0을 NAT Gateway ID(nat-gw-~)와 매핑
- ::/0는 송신 저용 인터넷 게이트웨이 아이디(eigw-id)와 매핑
###  VPC 요약
- CIDR은 IP 범위
- VPC는 가상 프라이빗 클라우드고 IPv4와 IPv6에 사용됨, 서브넷은 CIDR을 정의하는 AZ에 연결되고, 퍼블릭 서브넷과 프라이빗 서브넷이 있음
- 퍼블릿 서브넷은 인터넷 게이트웨이를 연결시켜 IPv4와 IPv6 인터넷 액세스를 제공
- 라우팅 테이블은 수정해야 하는 덷 인터넷 게이트웨이로의 경로들, VPC 피어링 연결, VPC 엔드포인트 등을 포함시켜 네트워크가 VPC 내에서 흐르도록 함
- Bastion Host는 퍼블릭 EC2 인스턴스고, SSH를 할 수 있음, 그 인스턴스는 프라이빗 서브넷에 있는 다른 EC2 인스턴스에 대한 SSH 연결성을 갖고 있음
- NAT 인스턴스는 프라이빗 서브넷에 있는 EC2 인스턴스에 인터넷 액세스를 제공하기 위해 퍼블릭 서브넷에 배포된 EC2 인스턴스 -> 작동을 위해 소스 대상 체크 플래그를 해제하고, 보안 그룹 규칙을 편집해야 함
- 대신에 NAT 게이트웨이를 쓸 수 있음 -> AWS에서 관리
	- 프라이빗 EC2 인스턴스에 확장 가능한 인터넷 액세스를 제공, IPv4만 사용할 수 있음
- NACL은 네트워크 ACL이고, 서브넷 수준에서 인바운드와 아웃바운드 액세스를 정의하는 방화벽 규칙, 안에는 임시 포트가 존재
- NACL은 상태가 없음(stateless), 그래서 인바운드와 아웃바운드 규칙이 항상 평가되는 반면에 보안 그룹 규칙은 상태를 갖고 있음(stateful)
	- stateful은 인바운드가 허용되면 아웃바운드가 자동으로 허용되고, 그 반대로도 허용
- 보안 그룹 규칙은 EC2 인스턴스 수준에서 적용됨
- VPC 피어링은 2개의 VPC를 연결할 때 유용하고, 중첩되지 않는 CIDR가 있을 경우에만 가능
- VPC 피어링 연결은 이행성이 없어서 3개의 VPC를 연결하려면  3개의 VPC 각각 피어링 연결이 필요
- VPC 엔드포인트를 이용해서 Amazon S3, DynamoDB, CloudFormation, SSM 등 원하는 AWS 서비스에  VPC 안에서 프라이빗 액세스를 할 수 있음
- <u>Amazon S3와 DynamoDB에 Gateway endpoint가 있고</u>, 나머지는 모두 <u>인터페이스 엔드포인트</u>
- VPC Flow Logs는 VPC 안의 모든 패킷에 관한 로그 수준 메타데이터를 얻는 방법
	- Allow와 Deny 트래픽에 관한 정보를 얻을 수 있음
- VPC Flow Logs는 VPC 서브넷 또는 ENA 레벨에서 생성되고 Amazon S3로 전송해서 Athena를 이용해 분석할 수 있으며 CloudWatch Logs로 전송해서 CloudWatch Log Insights를 이용해서 분석할 수 있음
- VPC를 데이터센터에 연결하는 방법은 두 가지
	- site-to-site VPN
		- 퍼블릭 인터넷을 통한 VPN 연결이라 AWS에 VPG(virtual private gateway)를 만들고, DC(Data center)에 Customer Gateway를 만들어서  VPN 연결을 수립
		- 여러개의 VPN 연결이 필요할 때 같은 VPG를 사용하면 VPN CloudHub를 사용해서 hub-and-spoke VPN 모델로 사이트들을 연결할 수 있음
	- Direct Connect
		- 연결은 <u>완전히 프라이빗</u> -> 퍼블릿 인터넷을 거치지 않지만 수립하는데 시간이 오래 걸림
		- 데이터센터를 Direct Connect 위치에 연결해야 함
		- 더 복잡하지만 더 안전하고 더 안정적
		- Direct Connect Gateway는 다른 AWS 리전의 많은 VPC와 있는 많은 Direct Connect를 하기 위한 것
- PrivateLink 또는 VPC 엔드포인트 서비스는 VPC 안의 서비스와 고객 VPC를 프라이빗하게 연결하기 위해 사용
- VPC 피어링 또는 퍼블릭 인터넷 또는 NAT 게이트웨이 또는 라우트 테이블이 필요하지 않음
	- 네트워크 로드 밸런서와 ENI와 함께 사용
	- VPC 내의 서비스를 네트워크에 노출하지 않고도 수백 또는 수천 개의 **고객 VPC에 노출**할 수 있음
- ClassicLink는 EC2-Classic 인스턴스를 VPC로 비공개로 연결하기 위한 것
- Transit Gateway는 VPC, VPN, 그리고 Direct Connect를 위한 transitive 피어링 연결
	- 모든 게  Transit Gateway를 통과해서 흐를 수 있다는 점이 유리
- Traffic Mirroring은 네트워크 트래픽을 ENI 복사해서 추가로 분석
- VPC의 IPv6
	- 송신 전용 인터넷 게이트웨이 필요
	- NAT 게이트웨이와 같지만 IPv6 트래픽을 인터넷으로 보내기 위한 것
### AWS의 네트워킹 비용
- 리전 하나에 가용 영역 두 개가 있는데 첫 번째 AZ에 EC2 인스턴스가 있다고 가정
	- EC2로 들어오는 트래픽은 모두 무료
	- 다른 EC2 인스턴스가 같은 가용 영역에 위치한다면 가용 영역은 지리적으로 인접한 여러 데이터 센터의 집합이 되므로 두 EC2 인스턴스 간 트래픽은 사설 IP로 통신할 시 전부 무료
- 다른 AZ에 있는 EC2 인스턴스까지 포함한다면 같은 리전 내 다른 두 AZ에 있는 EC2 인스턴스 두 개가 서로 소통하길 원할 경우 **공용 IP나 탄력적 IP**를 사용하는 방법
	- 청구되는 비용은 GB당 2센트
	- 두 인스턴스가 통신하려면 트래픽이 AWS 네트워크 밖으로 나갔다 다시 들어와야 하므로 AWS에서 비용을 청구
	- 대신 사설 IP를 사용하면 비용이 반으로 절감
	- 두 가용 영역을 연결하는 데 내부 AWS 네트워크를 사용하기 때문
- <u>인스턴스가 소통할 때 속도는 높이고 비용은 줄이려면 최대한 공용 IP보다는 사설 IP를 사용</u>
-  동일한 리전 내 가용 영역 두 곳에 있는 인스턴스 두 개가 통신할 때 공용 IP는 사용하지 않는 것을 추천
- 계산을 담당하는 클러스터가 있고, EC2 인스턴스 간에 활발히 소통하기를 요구한다면 최대한 비용을 절감하도록 같은 가용 영역을 사용해야 하지만 비용이 절감되는 만큼 가용성이 떨어짐 -> 장애조치 어려움
- 가용성과 비용 사이에서 균형을 유지해야 함
- RDS 데이터베이스가 있는데 읽기 전용 복제본을 생성하고, 분석을 진행하고 싶은데 읽기 전용 복제본을 가장 저렴하게 생성하는 방법은?
- 읽기 전용 복제본을 같은 AZ에 생성한다면 네트워크 비용 측면에서 볼 때 한 데이터베이스에서 다른 데이터베이스로 복제하는 데는 비용이 들지 않음
- 하지만 읽기 전용 복제본을 다른 AZ에 생성한다면 GB당 1센트를 지불해야 함
	- 두 데이터베이스 간 데이터를 수송하는 비용
- 송신 트래픽은 아웃바운드 트래픽으로 AWS에서 밖으로 나간다는 뜻
- 수신 트래픽은 인바운드 트래픽이고, 외부에서 AWS로 들어오는 트래픽으로 보통 무료
- AWS로 데이터를 보내는 건 대부분 무료지만 밖으로 데이터를 내보낼 땐 비용이 드니까 인터넷 트래픽을 최대한 AWS 내부에 두고 비용을 최소화해야 함
- 데이터베이스가 있고 기업 데이터 센터에 사용자가 있을 때 기업 데이터 센터에서 애플리케이션을 실행한다면 애플리케이션은 데이터베이스에 쿼리를 보내 데이터베이스에서 데이터 100MB를 회수한 뒤 집계와 계산까지 마치고 나면 사용자에게 쿼리 결과 50KB만을 반환한다고 가정
- 송신 트래픽이 많이 발생해서 비용도 아주 많이 듦
- EC2 인스턴스의 AWS Cloud로 애플리케이션을 이동하는 것이 해결책
	- 동일한 가용 영역에 데이터를 두기 때문에 DB 쿼리 데이터 전송이 무료
	- 100MB가 모든 계정에 생기지 않고, 쿼리 결과 50KB만 전송되기 때문에 비용이 훨씬 절감 -> **송신 비용 최소화**
- 송신 트래픽 네트워크 비용을 최소로 줄이는 또 다른 방법은 `Direct Connect`를 사용할 경우
- Direct Connect 로케이션을 동일한 AWS 리전으로 설정해 비용을 절감
- S3 데이터 전송 비용은 얼마나 될까?
- S3 버킷으로 들어가는 데이터는 수신 트래픽이므로 전부 무료이지만 Amazon S3에서 컴퓨터로 인터넷을 통해 데이터를 내려받는다면 송신 트래픽 1GB마다 9센트를 지불
- S3 전송 가속화(S3 Transfer Acceleration)를 사용한다면 전송 속도가 50-500% 정도 빨라지는데 데이터 전송에 GB당 4-8센트가 청구 추가 비용이 발생
	- 전송 가속화 기능은 비용이 듦
- S3에서 CloudFront로 발생하는 트래픽은 무료
- S3 버킷상에 CloudFront 배포를 생성하면 S3와 CloudFront 사이에서 데이터를 주고받는 건 무료
- 반면 CloudFront에서 인터넷으로 전송할 때는 GB당 8.5센트가 청구
	- Amazon S3보다는 약간 저렴하고, 캐싱 기능이 있음
	- 데이터 액세스 지연 시간이 짧아지며 요청을 보낼 때마다 비용이 절감
- S3 버킷에 요청이 들어오면 비용을 부담해야 하는데 Amazon CloudFront로 들어오면 일곱 배나 저렴
- Amazon S3 버킷에 리전 간 복제를 수행한다면 GB당 2센트를 지불
- 비용은 시간이 지나면서 혹은 지역에 따라 달라질 수 있음
- NAT Gateway와 게이트웨이 VPC 엔드 포인트 비용을 분석
	- 리전에 VPC가 있고 사설 서브넷 두 개는 각기 다른 유형의 EC2 인스턴스를 사용하고, 둘 다 Amazon S3 버킷으로 데이터를 액세스한다고 가정 -> 공용 인터넷 사용 추천
	- NAT Gateway가 있는 공용 서브넷을 생성해야 하는데 이때 인터넷 게이트웨이로 향하는 라우트가 필요
	- 라우팅 테이블을 사용해서 라우트를 구축하면 EC2 인스턴스에서 NAT Gateway와 인터넷 게이트웨이를 지나 인터넷과 직접 연결
	- Amazon S3 버킷 내 데이터를 인터넷에서 액세스할 수 있음
- NAT Gateway 요금은 시간당 0.045달러
	- NAT Gateway에서 처리되는 데이터 1GB마다 0.045달러가 들고, S3로 리전 간 데이터 송신 비용은 시간당 0.09달러가 청구되는데 동일한 리전이라면 비용이 발생하지 않음
- VPC 엔드 포인트가 있다면 Amazon S3 버킷에 비공개로 데이터를 액세스할 수 있고, 다른 라우팅 테이블을 생성
	- VPC 엔드 포인트에 액세스하려면 연결되는 라우트만 설치하면 됨
	- 데이터는 사설 서브넷에서 VPC 엔드 포인트와 S3 버킷으로 직접 흐르기 때문에 게이트웨이 엔드 포인트는 사용료가 들지 않음
- 동일한 리전에서 S3로 데이터를 주고받을 때 데이터 1GB마다 1센트를 지불하면 됨
- NAT Gateway보다 VPC 엔드 포인트를 쓰는 게 훨씬 저렴
### AWS 네트워크 파이어월
- 네트워크 액세스 제어 목록(NACL) Amazon VPC 보안 그룹 AWS WAF라는, HTTP를 통하여 특정 서비스에 유입되는 악성 요청을 막는 방화벽 
- DDoS를 막는 AWS Shield와 Shield Advanced가 있고, AWS Firewall Manager라는 여러 계정에 적용할 수 있는 WAF, Shield 등에 대한 규칙 관리자도 있음
- 전체 VPC를 보호할 수 있는 수준 높은 방법은?
	- AWS Network Firewall을 사용하면 됨
	- 전체 VPC를 방화벽으로 보호하는 서비스로 VPC가 있으면 이 전체에 방화벽을 두르는 것
- AWS Network Firewall은 계층 3에서 계층 7까지 보호하는데 모든 방향에서 들어오는 모든 트래픽을 검사
	- VPC 간 트래픽 인터넷 아웃바운드, 인터넷 인바운드, Direct Connect나 Site to Site VPN 연결까지 포함
	- 인터넷과의 모든 트래픽과 피어링된 VPC와의 모든 트래픽 Direct Connect와 Site to Site VPN을 오가는 모든 트래픽을 보호
	- 규칙을 정의하기만 하면 모든 동작을 제어할 수 있음
- 내부적으로 Network Firewall은 AWS Gateway Load Balancer를 사용
	- 타사 어플라이언스가 아니라 AWS에서 자체 어플라이언스를 통해 트래픽을 관리하므로 Network Firewall에 자체 규칙을 갖추고 있음
	- 규칙들 역시 중앙 집중식으로 관리되며 여러 계정과 VPC에 적용
- Network Firewall에서는 **네트워크 트래픽을 세부적으로 관리**할 수 있는데 VPC 수준에서 수천 개의 규칙을 지원하고, 수만 IP를 IP와 포트별로 필터링할 수 있음
	- 아웃바운드 통신에서는 SMB 프로토콜을 비활성화하는 식으로 프로토콜별로도 필터링할 수 있음
	- 도메인별로도 필터링할 수 있어서 VPC의 아웃바운드 트래픽에 대해서는 기업 도메인에만 액세스를 허용하거나 타사 소프트웨어 리포지토리에만 액세스를 허용할 수도 있음
	- regex 등을 이용해서 일반 패턴 매칭도 가능하고, 트래픽 허용, 차단, 알림 등을 설정해서 설정한 규칙에 맞는 트래픽을 필터링할 수도 있음
	- AWS가 관리하는 게이트웨이 로드 밸런서처럼 활성 플로우 검사를 통해 침입 방지 능력을 갖출 수 있음
- 모든 규칙 일치는 Amazon S3와 CloudWatch Logs 및 Kinesis Data Firehose로 전송해 분석할 수 있음
- **Network Firewall은 VPC 수준에서 설정할 수 있음**
- 트래픽 필터링과 플로우 검사를 지원
