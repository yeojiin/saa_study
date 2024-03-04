### IPSec Vpn 
- VPN의 AWS측에 가상 사설 게이트웨이(VGW)를 생성하고 VPN의 온프레미스 측에 고객 게이트웨이를 생성
- 구성
	- AWS Cloud > Region > VPC (Virtual Private gateway 있음) > AZ
		- -> AWS Site to Site VPN으로 Virtual Private gateway와 인터넷에 연결
	- 온프레미스 Client (IPSec VPN 있음)
		- -> IPSec VPN과 인터넷에 연결
### ElastiCache (for Redis)
- 읽기가 많은 어플리케이션 워크로드에 대한 대기 시간 및 처리량(throughput) 향상
- 컴퓨팅 집약적인(compute-intensive) 워크로드의 성능 향상
- 인메모리 데이터 스토어를 실행할 수 있음
- 캐싱, 세션 스토어, 게임, 지리 공간 서비스, 실시간 분석 및 대기열과 같은 실시간 사용 사례에 적합
- write에 대한 성능 향상이 되는 것은 아니고 `read의 성능 향상`
- ETL 워크로드는 캐싱에 적합하지 않은 `대용량 데이터`를 읽고 변환 -> AWS Glue 또는 Amazon EMR을 사용해야 함
### OAI(Origin Access Identity)
- cloudfront에서 인증된 사용자만 S3로 접근이 가능하도록 하는 방법은?
- OAI를 구성하고 CloudFront 배포판과 연결 후 OAI만 개체를 읽을 수 있도록 S3 버킷 정책에 권한을 설정
	- cloudfront는 S3에 인증된 요청을 보내도록 구성하고 cloudfront에서 이증된 요청에 대한 액세스만 허용하도록 S3를 구성
	- cloudfront가 S3 오리진에 인증된 요청을 보내는 방법
		- origin access control(OAC) -> 더 권장되는 방식
		- origin access identity(OAI)
- WAF ACL을 만들고 
---
### EFA ❓4번'
- EFA
	- HPC 및 기계 학습 어플리케이션을 가속화 하기 위한 네트워크 장치
	- 모든 ENA(Elastic Network Adapter) 장치와 하드웨어에서 제공하는 신뢰할 수 있는 전송 기능으로 사용자 공간 어플리케이션이 직접 통신할 수 있는 새로운 OS 바이패스 하드뒈어 인터페이스 제공
- ENI(Elastic network interface)
	- VPC의 논리적 구성 요소이며 가상 네트워크 카드를 의미함
	- **EC2인스턴스가 네트워크에 액세스 할 수 있게 해줌**
	- 인스턴스 외부에서도 사용 가능
	- 속성
		- 주요 private IPv4과 하나 이상의 secondary IPv4를 가질 수 있음
		- 하나의 엘라스틱 아이피 당 하나의 사설 IPv4 및 공용 IP가 한 개씩 제공
		- 하나 이상의 보안 그룹을 연결할 수 있음
		- EC2인스턴스와 독립적으로 ENI를 생성하고 즉시 연결하거나 장애 조치를 위해 EC2인스턴스에서 이동시킬 수 있음
			- EC2 1에 Eth0과 Eth1이 연결되고, EC22에 Eth0이 연결되어 있을 때
			- 1번이 장애가 생기면 2에 Eth1을 옮겨서 사설 IP가 1인스턴스에서 2인스턴스로 연결되게 할 수 있음 -> 장애조치에 유용
		- 특정 가용 영역인 AZ에 바인딩됨
- ENA
	- 고성능 네트워킹 기능을 제공하기 위해 SR-IOV(Single Root I/O Virtualization)를 통해 향상된 네트워킹을 지원
	- 향상된 네트워킹은 더 높은 대역폭, 더 높은 초당 패킷(PPS) 성능 및 지속적으로 더 낮은 인스턴스 간 지연 시간을 제공하지만 EFA 장치는 ENA 장치의 모든 기능과 확장 프로그래밍 인터페이스를 사용하여 인스턴스 커널(OS-bypass 통신) 없이 응용 프로그램이 EFA 장치와 직접 통신할 수 있도록 하드웨어를 지원하기 때문에 주어진 사용 사례에 더 적합
### NLB
- 라우팅 및 IP 주소 요청
- 인스턴스 ID를 사용하여 대상을 지정하면 해당 인스턴스의 기본 네트워크 인터페이스에 지정된 기본 개인 IP 주소를 사용해 트래픽이 인스턴스로 라우팅 됨
- IP 주소를 사용해 대상을 지정하면 하나 이상의 네트워크 인터페이스에서 임의의 개인 IP 주소를 사용해 트래픽을 인스턴스로 라우팅 할 수 있음
- 인스턴스 아이디로 선택할 수 없음
### Cloudformation StackSet
- Cloudformation StackSet은 한 번의 작업으로 여러 계정과 영역에 걸쳐 스택을 생성, 업데이트, 삭제 할 수 있도록 스택의 기능을 확장함
- 단일 cloudformation탬플릿을 사용해 영역에 거쳐 aws 계정에 스택을 생성할 수 있음
### ❓10 
- Launch Configuration LC1 및 Launch Configuration LC2에서 모두 시작된 인스턴스에 전용 인스턴스 테넌트가 있음
- launch configuration을 생성할 때 인스턴스 배치 tenancy의 기본값은 null이고, instance tenancy는 VPC 테넌트 특성에 의해 제어 됨
- If you set the Launch Configuration Tenancy to default and the VPC Tenancy is set to dedicated, then the instances have dedicated tenancy -> Launch Configuration 테넌트를 default로 설정하고 VPC 테넌트를 전용으로 설정하면 인스턴스는 전용 테넌트
- If you set the Launch Configuration Tenancy to dedicated and the VPC Tenancy is set to default, then again the instances have dedicated tenancy -> Launch Configuration 테넌트를 전용으로 하고 VPC 테넌트가 기본이면 인스턴스는 전용 테넌트
- 둘 중 하나만 전용이면 전용임?
### ❓15
- 알람을 사용하는 것은 ok, 근데 왜 EBS 볼륨만 사용?
	- 복구 작업은 EBS 볼륨이 구성된 인스턴스에서만 지원되고, 인스턴스 저장소 볼륨은 cloudwatch 경보에 의한 자동 복구를 지원하지 않음
- 인스턴스 상태 확인에 이벤트 브릿지로 복구할 수는 없음
### ❓19
- 호스트에서 왜 전용으로? 
###  AWS Config 
-  AWS Config를 사용해 리소스 구성을 검토하여 규정 준수 지침을 충족하고 리소스 구성 변경 기록을 유지할 수 있음
-  AWS Config는 리소스의 구성을 평가, 감사 할 수 있음
- 내부 지침에 명시된 구성에 대한 전반적인 준수 여부를 결정할 수 있음
- AWS CloudTrail을 사용하면 AWS 인프라 전체에서 작업과 관련된 계정 활동을 기록하고 지속적으로 모니터링하고 유지할 수 있습니다. AWS CloudTrail을 사용하여 "누가 이 리소스를 수정하기 위해 API 호출을 했습니까?"와 같은 질문에 답할 수 있음
- AWS CloudTrail은 AWS 계정 활동의 이벤트 기록을 제공하여 AWS 계정의 거버넌스, 규정 준수, 운영 감사 및 위험 감사를 가능하게 합니다. AWS CloudTrail을 사용하여 리소스 구성 변경 기록을 유지할 수 없음
- AWS CloudWatch는 애플리케이션을 모니터링하고 시스템 전체의 성능 변화에 대응하며 리소스 활용률을 최적화하고 통합된 운영 상태를 파악하기 위한 데이터와 실행 가능한 통찰력을 제공
### 
-  The company wants to provide shared and centrally-managed VPCs to all departments using applications that need a high degree of interconnectivity ->  고도의 상호 연결이 필요한 애플리케이션을 사용하여 모든 부서에 공유 및 중앙 관리 VPC를 제공하기를 원함
- Use VPC sharing to share one or more subnets with other AWS accounts belonging to the same parent organization from AWS Organizations
	 -> VPC 공유를 사용하여 AWS Organization의 동일한 상위 조직에 속한 다른 AWS 계정과 하나 이상의 서브넷 공유
- `VPC 공유`(Resource Access Manager의 일부)를 사용하면 여러 AWS 계정에서 Amazon EC2 인스턴스, Amazon RDS 데이터베이스, Amazon Redshift 클러스터 및 AWS Lambda 함수와 같은 응용 프로그램 리소스를 공유 및 중앙 관리되는 Amazon Virtual Private Cloud(VPC)로 생성할 수 있음
- 이를 설정하려면 VPC(소유자)를 소유한 계정이 AWS Organization에서 동일한 조직에 속한 다른 계정(참가자)과 하나 이상의 서브넷을 공유
- 서브넷이 공유된 후 참가자는 자신과 공유된 서브넷에서 자신의 응용 프로그램 리소스를 보고, 만들고, 수정 및 삭제할 수 있음
- 참가자는 다른 참가자 또는 VPC 소유자에게 속하는 리소스를 보고, 수정하거나 삭제할 수 없음
- Amazon VPC를 공유하여 고도의 상호 연결이 필요하고 동일한 신뢰 경계 내에 있는 애플리케이션에 대해 VPC 내의 암시적 라우팅을 활용할 수 있고, 이렇게 하면 과금 및 엑세스 제어를 위해 별도의 계정을 사용하면서 생성하고 관리하느 VPC의 수를 줄일 수 있음
- `VPC 피어링` 연결은 두 VPC 간의 네트워킹 연결로, 사설 IPv4 주소 또는 IPv6 주소를 사용하여 트래픽을 라우팅할 수 있음
-  두 VPC의 인스턴스는 동일한 네트워크 내에 있는 것처럼 서로 통신할 수 있음
- VPC 피어링은 중앙에서 관리되는 VPC를 용이하게 하지 않고, AWS 소유자 계정은 VPC 자체를 다른 AWS 계정과 공유할 수 없음
### AWS DMS & AWS SCT(AWS Schema Conversion Tool)
- 회사의 CTO는 보조 인덱스, 외부 키 및 저장된 절차와 같은 복잡한 데이터베이스 구성을 처리할 수 있는 솔루션
- AWS DMS
	-  데이터베이스를 AWS로 빠르고 안전하게 마이그레이션할 수 있음
	- 오라클에서 mysql 간 이종 마이그레이션도 지원
	- 이기종 마이그레이션은 먼저 AWS 스키마 변환 도구를 사용하여 소스 스키마와 코드를 대상 데이터베이스와 일치하도록 변환한 다음 AWS Database Migration Service를 사용하여 소스 데이터베이스에서 대상 데이터베이스로 데이터를 마이그레이션
	- 마이그레이션하는 동안 필요한 모든 데이터 유형 변환은 AWS Database Migration Service에 의해 자동으로 수행
	- 소스 데이터베이스는 AWS 외부의 온프레미스 환경에서 Amazon EC2 인스턴스에서 실행될 수도 있고 Amazon RDS 데이터베이스일 수도 있습니다. 대상은 Amazon EC2 또는 Amazon RDS의 데이터베이스일 수도 있음
	- 또한 소스 데이터베이스와 대상 데이터베이스 엔진이 다를경우 소스 데이터베이스와 대상 데이터베이스의 스키마 구조, 데이터 유형 및 데이터베이스 코드가 상당히 다를 수 있으므로 데이터 마이그레이션이 시작되기 전에 스키마와 코드 변환이 필요 -> AWS SCT(AWS Schema Conversion Tool)
### Amazon Macie
- Amazon GuardDuty는 S3에 저장된 AWS 계정, 워크로드 및 데이터를 지속적으로 모니터링하고 보호하는 위협 탐지 기능을 제공 -> 저장된 데이터에대한 모니터링을 하지 않음
- Amazon Macies는 기계 학습과 패턴 일치를 사용해 민감한 데이터를 검색하고, 데이터 보안 위험에 대한 가시성을 제공
### Dedicated hosts
- 애플리케이션 스택 전반에 걸쳐 여러 개의 장기 서버 바운드 라이센스를 보유하고 있으며 CTO는 AWS로 이전하는 동안 이러한 라이센스를 계속 활용하고 싶어함
- window server와 같은 `기존 서버 바인딩 소프트웨어 라이센스`를 사용할 수 있음
- 전용 인스턴스는 단일 고객 전용 하드웨어의 VPC에서 실행되는 인스턴스로 기존 서버 바인딩 소프트웨어 라이센스를 사용할 수 없음
### Connection Draining
- ELB에서 EC2 인스턴스로 가는 요청이 삭제되는 문제가 발생할 경우
- 기존 연결이 열려 있는 상태에서 등록 해제 중이거나 작동하지 않는 인스턴스에 대한 요청 전송을 중지하려면 connection draining(연결 제거)를 사용해야 함
- 이를 통해 로드 밸런서는 등록 해제 중이거나 작동하지 않는 인스턴스에 대한 inflight 요청을 대기했다가 완료할 수 있음
### scale out
- ALB를 사용하는데 트래픽이 급증할 때마다 클라우드워치 알람이 가용성 제약이 있을 때마다 알림을 받도록 구성해 리소스를 확장할 경우
- threshol(임계값) 이상으로 CPU사용량이 늘어나면 scale out -> 클라우드 워치의 임계값이 늘어나는 알람을 받는게 아니라 EC2의 CPU 사용량을 받아야 함
### Kinesis Data Stream
- real-time health data of -> 실시간 데이터
### FSx
- IT 회사는 사내 데이터 센터에서 윈도우즈 기반 애플리케이션을 호스팅합니다. 회사는 AWS 클라우드로 비즈니스를 이전하려고 합니다. 클라우드 솔루션은 여러 애플리케이션이 복제할 필요 없이 액세스할 수 있는 공유 스토리지 공간을 제공해야 하고, 회사의 자체 관리되는 Active Directory 도메인과 통합되어야 하는 경우
- `윈도우즈 파일 서버` 는 FSx
### AWS DataSync
- 내 데이터를 쉽고 빠르고 비용 효율적으로 Amazon S3, Amazon Elastic File System(Amazon EFS) 및 Amazon FSx for Windows File Server로 **이동**할 경우
- AWS DataSync는 인터넷 또는 AWS DirectConnect를 통해 AWS 스토리지 서비스에 대용량 데이터를 단순화, 자동화 및 신속하게 복사하는 온라인 데이터 전송 서비스
-  기본적으로 Amazon S3, Amazon EFS, Amazon FSx for Windows File Server, Amazon CloudWatch 및 AWS CloudTrail과 통합되어 스토리지 서비스에 대한 원활하고 안전한 액세스와 전송에 대한 상세한 모니터링을 제공
- AWS DataSync는 데이터 전송을 완전히 자동화
- `AWS Storage Gateway`의 파일 인터페이스 또는 파일 게이트웨이는 클라우드에 연결하여 애플리케이션 데이터 파일과 백업 이미지를 Amazon S3 클라우드 스토리지에 내구성 있는 개체로 저장할 수 있는 원활한 방법을 제공
- File Gateway는 로컬 캐싱을 통해 Amazon S3의 SMB 또는 NFS 기반 데이터에 대한 액세스를 제공
- 내 애플리케이션과 S3 개체 스토리지에 대한 파일 프로토콜 액세스가 필요한 Amazon EC2 기반 애플리케이션에 사용할 수 있음
- EFS 및  Amazon FSx for Windows File Server 로의 마이그레이션에 적합하지 않음
### Multi-AZ 
- DB 운영을 중단할 때 데이터 손실을 최소화하고 모든 트랜잭션을 최소 2개의 노드에 저장하는 AWS의 신뢰할 수 있는 데이터베이스 솔루션으로 마이그레이션할 경우
- Multi-AZ 기능을 사용하여 데이터를 동기적으로 복제할 수 있는 Amazon RDS MySQL DB 인스턴스 설정
- RDS Multi-AZ DB Instance를 프로비저닝할 때 Amazon RDS는 자동으로 기본 DB 인스턴스를 생성하고 데이터를 다른 AZ(Availability Zone)의 대기 인스턴스에 동기적으로 복제
-  DB 인스턴스 장애 및 가용성 영역 중단으로부터 데이터베이스를 보호할 수 있음
- 계획되거나 계획되지 않은 DB 인스턴스 중단이 발생할 경우 Multi-AZ를 사용하도록 설정한 경우 Amazon RDS는 자동으로 다른 가용성 영역의 대기 복제본으로 전환됨
### report generation tool/application
- business reports 보고서가 매우 느릴 경우
-  읽기량이 많은 데이터베이스 워크로드에 대해 단일 DB 인스턴스의 용량 제약을 넘어 쉽게 확장할 수 있음
- DB 인스턴스의 복제본을 하나 이상 생성하고 데이터의 여러 복사본에서 대용량 애플리케이션 읽기 트래픽을 처리하여 집계 읽기 처리량을 늘릴 수 있음
- 읽기 복제본은 독립 실행형 DB 인스턴스가 되어야 할 때 승격할 수도 있음
### AWS Database Migration Service (AWS DMS)
- 일일 보고서를 작성하기 위해 아마존 RDS에서 오라클 및 PostgreSQL 서비스에 대한 임시 쿼리를 실행해 왔습니다. 이제 엔지니어링 팀은 비즈니스 분석 보고서 작성을 용이하게 하기 위해 이 데이터를 지속적으로 복제하고 이러한 데이터베이스를 아마존 레드시프트로 스트리밍하여 페타바이트 규모의 데이터 웨어하우스로 통합할 경우
- WS Database Migration Service를 사용하면 Amazon Redshift와 Amazon S3로 데이터를 스트리밍하여 가용성이 높은 데이터를 지속적으로 복제하고 데이터베이스를 페타바이트 규모의 데이터 웨어하우스로 통합
- AWS Glue은 고객이 분석을 위해 데이터를 쉽게 준비하고 로드할 수 있도록 하는 ETL(완전 관리형 추출, 변환 및 로드) 서비스 -> 배치 ETL 작업에 용이, 사용자정의 마이그레이션 스크립트 작성하는데 공수가 필요함
### NAT
- 공용 서브넷과 사설 서브넷. 팀은 공용 서브넷에 있는 NAT(네트워크 주소 변환) 인스턴스 또는 NAT(네트워크 주소 변환) 게이트웨이를 사용하여 사설 서브넷의 인스턴스가 인터넷으로 아웃바운드 IPv4 트래픽을 시작할 수 있도록 지원하려고 하지만 NAT(네트워크 주소 변환) 인스턴스 및 NAT(네트워크 주소 변환) 게이트웨이에 사용할 수 있는 구성 옵션과 관련하여 일부 기술 지원이 필요할 경우
- NAT instance는 bastion server에 있어야 하고 -> NAT GW는 아님
- Security Groups can be associated with a NAT instance
- NAT instance supports port forwarding
### AWS Global Accelerator
- 게임 회사는 다양한 서비스와 마이크로서비스를 위해 Amazon EC2 인스턴스 앞에 Application Load Balancers를 사용합니다. 여러 AWS 영역에서 너무 많은 Application Load Balancers로 아키텍처가 복잡해졌습니다. 보안 업데이트, 방화벽 구성 및 트래픽 라우팅 로직은 너무 많은 IP 주소 및 구성으로 복잡해졌습니다. 그 회사는 방화벽에 의해 허용되는 IP 주소의 수를 줄이고 전체 네트워크 인프라를 쉽게 관리할 수 있는 쉽고 효과적인 방법을 찾고 있습니다. 이 요구 사항에 대한 적절한 해결책을 나타내는 옵션은 무엇입니까?
- `AWS Global Accelerator`를 시작하고 모든 지역에 대한 엔드포인트를 생성합니다. 각 지역의 애플리케이션 로드 밸런서를 해당 엔드포인트에 등록
- AWS 글로벌 액셀러레이터는 아마존 웹 서비스의 글로벌 네트워크 인프라를 통해 사용자의 트래픽을 전송하는 네트워킹 서비스로 인터넷 사용자 성능이 최대 60% 향상
- 인터넷이 혼잡할 때 글로벌 액셀러레이터의 자동 라우팅 최적화는 패킷 손실, 지터 및 지연 시간을 지속적으로 낮게 유지하는 데 도움
- AWS Global Accelerator를 사용하면 트래픽 관리를 단순화할 수 있는 두 개의 글로벌 정적 고객 대면 IP를 제공받을 수 있음
- 백엔드에서는 사용자 대면을 변경하지 않고 네트워크 로드 밸런서, 애플리케이션 로드 밸런서, 탄력적 IP 주소(EIP) 및 Amazon EC2 인스턴스와 같은 AWS 애플리케이션 오리진을 추가하거나 제거
- 엔드포인트 장애를 완화하기 위해 AWS Global Accelerator는 트래픽을 가장 가까운 정상적인 사용 가능한 엔드포인트로 자동으로 재라우팅
- `ASG`는 Amazon EC2 인스턴스 모음만 관리하므로 ASG(Auto Scaling Group)에 탄력적인 IP 주소를 할당할 수 없음
### Instance Recovery
- 아마존 EC2 인스턴스가 손상되면 아마존 클라우드워치 경보를 사용하여 자동으로 복구하려고 할 경우
- 복구된 인스턴스는 인스턴스 ID, 개인 IP 주소, Elastic IP 주소 및 모든 인스턴스 메타데이터를 포함하여 원래 인스턴스와 동일
- 인스턴스에 공용 IPv4 주소가 있는 경우 복구 후 공용 IPv4 주소를 유지
- 아마존 EC2 인스턴스가 기본 하드웨어 장애 또는 복구에 AWS 관련 문제로 인해 손상된 경우 Amazon CloudWatch 경보를 생성하여 자동으로 복구할 수 있음
- 종료된 인스턴스는 복구 불가
- 인스턴스 복구 중에 인스턴스는 인스턴스 재부팅 중에 마이그레이션 되고 메모리에 있는 데이터는 손실됨
### Service control policy (SCP)
- AWS Organization(AWS Organization)은 조직의 모든 계정에 대해 사용 가능한 최대 권한을 중앙에서 제어하기 위해 SCP(Service Control Policies)를 사용하고 있는 경우
- 사용자 또는 SCP에 의해 허용되지 않거나 명시적으로 거부되는 작업  IAM 정책이 있을 경우 수행불가
- SCP는 구성원 계저의 루트 사용자를 포함해 구성원 계정의 모든 사용자 및 역할에 영향을 미침
- SCP는 서비스 연결 역할(service-linked role)에 영향을 주지 않음
### Alias subdomain
- 아마존 루트 53을 사용하여 covid19survey.com 도메인을 구입했습니다. 웹 개발 팀은 covid19survey.com 의 모든 트래픽이 www.covid19survey.com 로 라우팅되도록 아마존 루트 53 레코드를 만들고자 할 경우
- 별칭 레코드를 만들어야 함
- 별칭 레코드를 사용하면 Amazon CloudFront 배포판 및 Amazon S3 버킷과 같은 선택된 AWS 리소스로 트래픽을 라우팅할 수 있음
-  top node of a DNS 인  DNS 네임스페이스의 맨 위 노드에 별칭 레코드를 만들 수 있지만 DNS 네임스페이스의 맨 위 노드에 대한 CNAME 레코드를 만들 수는 없음
- 따라서 DNS 이름 covid19survey.com 을 등록하면 존 정점은 covid19survey.com
- covid19survey.com 에 대한 CNAME 레코드를 만들 수는 없지만 www.covid19survey.com 에 트래픽을 라우팅하는 covid19survey.com 에 대한 별칭 레코드를 만들 수는 있음
- top node라서 CNAME으로 만들 수 없음
### endpoint
- 온프레미스 데이터 센터를 AWS Direct Connect를 통해 AWS Cloud에 연결했습니다. 이 회사는 온프레미스 네트워크의 모든 리소스에 대한 DNS 쿼리를 AWS VPC에서 해결하고 온프레미스 네트워크의 리소스에 대한 DNS 쿼리도 해결하기를 원할 경우
- Amazon Route 53 Resolver에서 인바운드 엔드포인트를 생성한 다음 온프레미스 네트워크의 DNS Resolver에서 이 엔드포인트를 통해 DNS 쿼리를 Amazon Route 53 Resolver로 전달할 수 있음
- Amazon Route 53 Resolver에 아웃바운드 끝점을 생성한 다음 Amazon Route 53 Resolver는 이 엔드포인트 통해 온프레미스 네트워크의 Resolver에 조건부로 쿼리를 전달할 수 있음
- Amazon Route 53은 사용자 요청을 Amazon EC2 인스턴스와 같이 AWS에서 실행되는 인프라에 효과적으로 연결하고 사용자를 AWS 외부의 인프라로 라우팅하는 데에도 사용할 수 있음
- Amazon Route 53 Resolver는 Amazon EC2 인스턴스의 로컬 VPC 도메인 이름에 대한 DNS 쿼리에 자동으로 응답
- 전달 규칙을 구성하여 사내 네트워크에서 Resolver와 DNS Resolver 간의 DNS 확인을 통합할 수 있음
- 온프레미스 네트워크에서 AWS VPC의 리소스에 대한 DNS 쿼리를 해결하려면 아마존 루트 53 레졸버에서 인바운드 엔드포인트를 생성한 다음 온프레미스 네트워크의 DNS 레졸버에서 이 엔드포인트를 통해 DNS 쿼리를 아마존 루트 53 레졸버로 전달할 수 있음
### Spot instance
- 특정 야간 배치 작업을 인스턴스를 발견하기 위해 이동하려고 할 경우
- 스팟 요청이 지속되면 스팟 인스턴스가 중단된 후 다시 열림
	- 요청이 지속적이고 스팟 인스턴스를 중지하면 스팟 인스턴스를 시작한 후에만 요청이 열림
- Spot Instance 요청이 활성화되어 있고 실행 중인 Spot Instance가 연결되어 있거나, Spot Instance 요청이 비활성화되어 있고 관련 중지된 Spot Instance가 있는 경우 요청을 취소해도 인스턴스가 종료되지 않으므로 실행 중인 Spot Instance를 수동으로 종료해야 함
- 또한 지속적인 Spot 요청을 취소하고 Spot Instance를 종료하려면 먼저 Spot 요청을 취소한 다음 Spot Instance를 종료해야 함
### AZ ID❓54
-  두 개의 AWS 계정이 있으며 모든 리소스는 us-west-2 지역에 프로비저닝되어 있습니다. IT 컨설턴트는 두 개의 AWS 계정에서 각각 Amazon EC2 인스턴스를 시작하여 인스턴스가 us-west-2 지역의 동일한 AZ(Availability Zone)에 있도록 하려고 합니다. 각 AWS 계정에서 인스턴스를 시작하는 동안 동일한 기본 서브넷(us-west-2a)을 선택한 후에도 IT 컨설턴트는 AZ(Availability Zone)가 여전히 다르다는 것을 알게 된 경우
- 가용성 영역(AZ) ID를 사용하여 두 AWS 계정에서 가용성 영역을 고유하게 식별
- 리소스가 지역의 Availability Zone에 분산되어 있는지 확인하기 위해 AWS는 Availability Zone을 각 AWS 계정의 이름에 매핑해서 예를 들어, 한 AWS 계정의 Availability Zone us-west-2a는 다른 AWS 계정의 us-west-2a와 동일한 위치가 아닐 수 있음
- 계정 간에 가용 영역을 조정하려면 가용 영역에 대한 고유하고 일관된 식별자인 AZ ID를 사용해야 합니다. 예를 들어 usw2-az2는 us-west-2 영역의 AZ ID이며 모든 AWS 계정에서 동일한 위치를 가짐
- AZ ID를 확인하면 다른 계정의 리소스와 비교하여 한 계정의 리소스 위치를 확인할 수 있습니다. 예를 들어 AZ ID가 usw2-az2인 가용성 영역의 서브넷을 다른 계정과 공유하는 경우 이 서브넷은 가용성 영역의 해당 계정에서 사용할 수 있으며 AZ ID도 usw2-az2임
- 기본 VPC를 사용하여 두 AWS 계정에서 가용 영역을 고유하게 식별하는 것은 다른 VPC와 논리적으로 격리되어 있어 불가 -> VPC는 AWS 영역에 걸쳐 있기 때문에 가용 영역을 고유하게 식별하는 데 사용할 수 없음
### FileGateway ,FSx
- 데이터 센터에서 윈도우즈 파일 서버 클러스터를 이전하려고 합니다. 그들은 완벽한 윈도우즈 호환성을 제공하는 클라우드 파일 스토리지 제품을 찾고 있습니다. 윈도우즈 시스템과 호환되는 업계 표준 SMB(Server Message Block) 프로토콜을 통해 액세스할 수 있는 매우 안정적인 파일 스토리지를 제공하는 AWS 스토리지 서비스를 식별할 수 있습니까?
- FileGateway ,FSx
### Dynamo DB ❓56
 - NoSQL persistent data store, 낮은 지연시간
 - DAX는 DynamoDB 호환 캐싱 서비스로 까다로운 애플리케이션에서 빠른 인메모리 성능의 이점을 누릴 수 있음 
 - RDS는 NoSQL이 아님
 - 영구적인 저장이 가능?
### Internet GW(I1)💡
-  DevOps 팀이 사용자 지정 VPC(V1)를 만들고 인터넷 게이트웨이(I1)를 VPC에 연결했습니다. 팀은 또한 이 사용자 지정 VPC에 서브넷(S1)을 만들고 이 서브넷의 경로 테이블(R1)에 인터넷 인바운드 트래픽을 인터넷 게이트웨이로 지시하는 경로를 추가했습니다. 이제 팀은 서브넷 S1에서 Amazon EC2 인스턴스(E1)를 시작하고 이 인스턴스에 공용 IPv4 주소를 할당합니다. 다음 팀은 서브넷 S1에서 네트워크 주소 변환(N1) 인스턴스도 시작합니다.   주어진 인프라 설정에서 Amazon EC2 인스턴스 E1에 대한 네트워크 주소 변환을 수행하는 엔티티는 다음 중 어느 것입니까?
- 인터넷 게이트웨이는 VPC와 인터넷 간의 통신을 허용하는 수평적으로 확장되고 중복되며 가용성이 높은 VPC 구성 요소
-   인터넷 게이트웨이는 인터넷 라우팅 가능한 트래픽에 대한 VPC 경로 테이블의 대상을 제공하고 공용 IPv4 주소가 할당된 인스턴스에 대해 네트워크 주소 변환(NAT)을 수행하는 두 가지 목적을 수행합니다. 따라서 예를 들어 E1과 같은 네트워크 주소 변환은 인터넷 게이트웨이 I1에 의해 수행
- VPC의 서브넷에 있는 인스턴스에 대해 인터넷에 액세스하거나 인터넷에서 액세스할 수 있도록 하려면 다음 작업을 수행
	- VPC에 인터넷 게이트웨이를 연결
	- 서브넷의 경로 테이블에 인터넷 연결 트래픽을 인터넷 게이트웨이로 유도하는 경로를 추가
	- 서브넷이 인터넷 게이트웨이로 가는 경로가 있는 경로 테이블과 연결되어 있으면 공용 서브넷이라고 합니다. 서브넷이 인터넷 게이트웨이로 가는 경로가 없는 경로 테이블과 연결되어 있으면 개인 서브넷
	-   서브넷의 인스턴스에 전역적으로 고유한 IP 주소(공용 IPv4 주소, Elastic IP 주소 또는 IPv6 주소)가 있는지 확인
	- 네트워크 액세스 제어 목록 및 보안 그룹 규칙을 통해 관련 트래픽이 인스턴스로 오고 갈 수 있는지 확인
### Aurora Global DB
- 분산 애플리케이션을 설계하고 있습니다. 하나의 AWS 지역에서 예약된 주문은 1초 이내에 모든 AWS 지역에서 볼 수 있어야 합니다. 데이터베이스는 짧은 RTO(Recovery Time Objective)를 통해 페일오버를 용이하게 할 수 있어야 할 경우
- 오로라 글로벌 데이터베이스는 기본 오로라 DB 클러스터에서 제공하는 페일오버보다 더 포괄적인 페일오버 기능을 제공
- 오로라 글로벌 데이터베이스를 사용하면 재해에 대한 계획과 복구를 상당히 빠르게 수행할 수 있음
- 재해 복구는 일반적으로 RTO 및 RPO 값을 사용하여 측정
- `DynamoDB` 는 DynamoDB 글로벌 테이블은 고성능의 지역 간 활성화 기능을 제공하지만 SQL 기반 데이터베이스와 함께 제공되는 일부 데이터 액세스 유연성을 잃게 됨
- DynamoDB 글로벌 테이블은 액티브-액티브 구성으로 인해 애플리케이션이 해당 지역의 테이블에 기록한 다음 데이터가 복제되어 다른 지역의 테이블을 동기화 상태로 유지하기 때문에 페일오버의 개념이 없음
###  AWS Directory Service (AWS Managed Microsoft AD)
- SQL Server 기반 애플리케이션을 위해 AWS에서 디렉토리 인식 워크로드를 실행하기를 원하고, 용자가 두 도메인의 리소스에 액세스할 수 있도록 SSO(Single Sign-On)를 사용할 수 있도록 신뢰 관계를 구성할 경우 
- AWS Directory Service for Microsoft Active Directory (AWS Managed Microsoft AD)
- AWS Directory Service for Microsoft Active Directory(일명 AWS Managed Microsoft AD)는 AWS에서 관리하는 실제 Microsoft Windows Server Active Directory(AD)를 기반
- AWS Managed Microsoft AD를 사용하면 SQL Server 기반 응용 프로그램과 같은 AWS 클라우드에서 디렉토리 인식 워크로드를 실행할 수 있음
-  AWS 클라우드의 AWS Managed Microsoft AD와 기존 온프레미스 Microsoft Active Directory 간의 신뢰 관계를 구성하여 단일 SSO(Single Sign-On)를 사용하여 사용자와 그룹이 두 도메인의 리소스에 액세스할 수 있도록 제공할 수 있음
### CNAME Record
- 응용 프로그램은 yourapp.provider.com 의 제공자에 의해 호스팅됩니다. 귀하는 아마존 루트 53에 따라 소유하고 관리하는 www.your-domain.com 을 사용하여 사용자가 귀하의 응용 프로그램에 액세스하도록 하기를 원할 경우 
