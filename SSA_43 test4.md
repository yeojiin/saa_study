###  Multi-AZ
- Multi-AZ deployment 배포 환경에서 RDS for SQL Server DB로 마이그레이션
- Multi-AZ deployment는 `Microsoft SQL Server`를 지원
- 수동지원 없이 서비스 중단 시 RDS는 자동으로 최신 보조 DB 인스턴스로 페일 오버함
- 이전과 같은 엔드포인트를 사용해 어플리케이션을 재구성할 필요 없음
### Direct Connect 2번❓
- 사내 데이터 센터와 AWS간 dedicated private connection을 지원하고 싶고, 장애 발생 시 가동 시간 보장, 암호화된 연결을 위한 공공 인터넷을 사용할 경우?
- Direct Connect 사용 시 전용 네트워크 연결 가능하나 인터넷을 포함하지 않고, 대신 인트라넷과 VPC 사이의 전용 사설 네트워크 연결을 사용
- Direct Connect를 기본 연결로 사용하면(연결이 비공개이기 때문) 성능 보장
- Direct Connect는 백업 솔루션으로 작동
- 공용 인터넷을 설치하기 위해 Site-to-Site VPN으로 백업을 연결
- 인터넷 기반 연결을 사용하는 것을 페일오버 시나리오에만 해당하므로 primary 연결로 Site-to-Site VPN 연결은 권장되지 않음
- Direct Connect는 안전하고 물리적인 연결이며 비용이 많이 들기 때문에 백업에 연결하는것은 적합하지 않음
### Spot instance
- 이미지를 압축하고 사용자는 결과를 기다릴 수 있음, 작업을 비동기식(asynchronously)으로 처리하고 빠른 확장을 원하면 실패 시 다시 시도를 원할 경우?
- 볼륨의 예측 불가능한 특성과 비용 절약을 위해 스팟 인스턴스 사용 권장
- SQS로 비동기 처리 후 다시 시도도 가능
- 예약된 인스턴스는 스팟보다 비용이 높음 -> 여기선 서비스가 떨어져도 괜찮은 경우
### MQ
- Amazon MQ는 Apache ActiveMQ를 위한 관리형 메세지 브로커 서비스
### EC2
- RDS 데이터베이스에서 매일 오전 7시에 일괄 작업을 실행하는데 지난 하루 동안의 배송 주문을 처리하고, shell script를 통해 일괄 작업으로 약 2000개의 레코드를 받으며 각 3초가 걸릴 때 추천하는 배치 작업은?
- AWS 배치는 아마존 EC2 인스턴스에서 배치 컴퓨팅 워크로드를 계획, 예약 및 실행하는 데 사용할 수 있습니다. 아마존 EC2는 배치 처리를 수용
- Glue는 shell script 실행 불가, 고객이 분석을 위해 데이터를 준비하고 로드하는 ETL서비스
- 람다는 최대 15분까지 실행되도록 구성됨, 2000\*\3=6000로 100분이 걸려 적합하지 않음
### ASG
-  애플리케이션 로드 밸런서와 ASG(Auto Scaling Group)로 구성된 고전적인 2계층 아키텍처를 설정했을 때 애플리케이션 로드 밸런서는 크기 10.0.1.0/24의 서브넷에 배포되고 자동 스케일링 그룹은 크기 10.0.4.0/22의 서브넷에 배포됨, 보안그룹이 ALB에서 들어오는 트래픽만 허용하려고 할 경우?
- How do you configure the security group of the Amazon EC2 instances to only allow traffic coming from the Application Load Balancer?
- Add a rule to authorize the security group of the Application Load Balancer
	- ALB의 보안그룹을 승인하는 규칙 추가
- ASG에 ALB의 보안그룹 승인 규칙을 추가해야 함 -> ASG입장에서 추가할 규칙 선택, 해석 오류
### SSL for data in transit
- RDS PostgreSQL를 DB로 사용하고 EC2 인스턴스의 인바운트 트래픽을 허용, KMS 사용 시 end-to-end secure access를 용이하게 하는 단계는?
- SSL for data in transit 전송 중 데이터에 SSL을 사용하도록 RDS를 구성
- Amazon RDS는 인스턴스가 프로비저닝되면 SSL 인증서를 생성하고 인증서를 DB 인스턴스에 설치해 PostgreSQL DB 인스턴스 간의 PostgreSQL 연결을 암호화할 수 있음
### AWS 데이터베이스 마이그레이션 서비스(AWS DMS)
-  빅 데이터 분석 회사는 아마존 S3 버킷에 데이터와 로그 파일을 작성, kinesis data stream으로 진행중인 파일 업데이트도 스트리밍하고 싶을 경우 가장 빠른 방법은?
- AWS 데이터베이스 마이그레이션 서비스(AWS DMS)를 S3와 kinesis data stream 간의 브릿지로 사용
- AWS DMS를 사용하면 지원되는 소스에서 AWS 클라우드의 관계형 데이터베이스, 데이터 웨어하우스, 스트리밍 플랫폼 및 기타 데이터 스토어로 데이터를 원활하게 마이그레이션할 수 있음
- 최소 시간 내에 이루기 위해선 DMS 사용 
	- 새로운 코드를 작성하거나 유지 관리하지 않아도 됨
- 람다 사용 시 개발이 추가로 필요하기 때문에 옳지 않음
### DAX
- 서버리스로 구축한 서비스에서 DynamoDB는 RCU(읽기 용량)을 늘렸지만 핫 파티션 문제가 발생함, 리펙토링 없이 해결하는 방법은?
- DAX(DynamoDB Accelerator)은 가용성 높은 인메모리 캐시로 최대 10배의 성능 향상을 제공함
- 개발자의 관리 없이 테이블에 인메모리 가속화를 추가할 수 있고, hotkeys를 제공해 캐시하기 때문에 빠름
- Amazon ElastiCache는 Lambda 측에서 리펙토링 작업이 필요함
### on-demand DynamoDB 
- DynamoDB를 사용하는데 밤에는 사용되지 않고, 낮에는 예측할 수 없는 트래픽이 발생함
- 알 수 없는 워크로드로 새 테이블을 만들 경우
- 예측할 수 없는 트래픽이 많은 경우 온디멘드 -> 프로비전드는 예측 가능할 때
- Dynamo 자동 스케일링은 프로비전드 모드에서 활용
### NLB
- Network Load Balancers는 보안 그룹을 지원하지 않으므로 대상 그룹 구성에 따라 클라이언트의 IP 주소 또는 Network Load Balancers와 연결된 개인 IP 주소가 웹 서버의 보안 그룹에서 허용어야 함
	- IP 화이트리스트 사용 가능
- ALB는 HTTP, HTTPS 등으 프로토콜 사용
### AWS Global Accelerator
- AWS Global Accelerator에 여러 지역의 Application Load Balancers를 등록
- AWS Global Accelerator와 연결된 정적 IP 주소를 허용하도록 온프레미스 방화벽의 규칙을 구성해야 함
### EKS, ECS
- 도커 컨테이너의 호스팅을 용이하기 위한 orchestration 서비스는?
	- ECS를 사용하면 로드밸런서 대신 API Gateway 및 Cloud Map(검색에 사용)으로 서비스를 노출할 수 있음
- EMR은 기업 연구원, 데이터 분석가에게 방대한 데이터를 쉽고 효율적으로 처리하도록 지원하는 웹서비스, Hadoop 등
### CloudFront
- CDN을 사용하고 Certified Solutions Architect – Associate로 고용하여 라우팅, 보안 및 고가용성(HA)에 대한 아마존 클라우드프론트 기능을 쓸 경우?
- Amazon CloudFront는 콘텐츠 유형에 따라 여러 오리진으로 라우팅
	- 로드밸런서의 동적 콘텐츠, 정적 컨텐츠 등 구분
- 필드 레벨 암호화를 사용해 특정 컨텐츠 보안
	- 필드 수준 암호화를 진행하면 사용자의 중요한 정보를 안전히 업로드 가능
	- KMS는 특정 콘텐츠만 보호하는 것이 아님
### EventBridge
- SaaS 어플리케이션과 사내 어플리케이션을 마이그레이션 하고 싶을 경우 비동기적으로 분리?
- SaaS와 통합되는 유일한 서비스
- SQS는 타사 SaaS와 통합되지 않음
### Instance Store
- 데이터를 캐시하고 최고 성능의 로컬 디스크 필요, EC2 복제 시 종료되거나 중지된 인스턴스가 데이터를 잃어버리는 것을 허용
- 인스턴스에 대해 임시 블록 수준의 저장소를 제공 -> 데이터 잃어도 괜찮기 때문
- EBS는 잃어버리지 않음
### Secure Sockets Layer certificate (SSL certificate) with SNI
- 다른 URL이 다른 대상 그룹과 연결된 로드밸런서를 사용, 보안을 위해 URL을 HTTPS 엔드포인트로 노출
- 하나의 로드 밸런서 뒤에 각각 고유의 TLS 인증서를 가진 여러 개의 TLS 보안 응용 프로그램을 호스팅할 수 있음
-  SNI를 사용하려면 로드 밸런서의 동일한 보안 청취자에게 여러 개의 인증서를 바인딩하기만 하면 됨 
- ALB는 자동으로 각 클라이언트에 맞는 최적의 TLS 인증서를 선택
### Pilot Light 
- 최소한의 비용으로 재해 복구 하는데 15분의 데이터 손실 감수
- 백업 인프라의 작은 부분은 중요한 데이터의 손실이 없도록 항상 동시에 변경 가능한 데이터를 동기화함
- 10분이내로 완료
- 중요한 건 계속 살리고 나머진 빠르게 프로비저닝 가능
- warm standby는 완전한 기능을 갖춘 환경의 축소판이 클라우드에 항상 실행됨
### Snowball Edge Compute Optimized
- 여러 Snow Family 기기를 사용해 사내 데이터를 이동할 때 스코리지 클러스터링 기능을 제공하는 것은?
- Snowball Edge Compute Optimized는 컴퓨팅 최적화와 스토리지 최적화 두 가지 장치 옵션으로 제공됨
- 간헐적 연결과 원격 연결이 가능한 두가지를 같이 사용
- 클러스터링 기능 제공
### CloudFormation
- RDS best practices 모범 사례를 재사용 가능한 템플릿으로 DB를 생성할 수 있도록 하는 경우?
- 템플릿으로 사용 가능 -> 해석 오류
### Amazon Neptune
- 데이터 세트와 함께 작용
- redshift는 클라우드 기반 데이터 웨어 하우스
### Dynamo DB Streams + Lambda
- 알림 전송 지연을 최소화하는 DynamoDB 기능은?
- Dynamo DB Streams를 활성화 하면 테이블의 데이터 항목에 대한 모든 수정 정보를 캡처
- 모든 변경사항에 대해 할 수 있고 람다로 전달
### Multi-AZ
- ElastiCache Redis를 사용하고 최소한의 다운타임과 최소한의 데이터 손실을 보장하는것은?
- Multi-AZ를 사용하면 데이터 손실 방지
### CloudFront distribution
- 콘텐츠는 현재 Amazon EFS에서 호스팅되고 ELB(Elastic Load Balancing) 및 Amazon EC2 인스턴스에 의해 배포, 웹 사이트는 매월 높은 부하와 매우 높은 네트워크 비용을 경험 솔루션 설계자로서 애플리케이션을 강제로 리팩터하고 네트워크 비용과 Amazon EC2 로드를 크게 줄이지 않는 방법을 추천
- Amazon CloudFront Points of Presence (POPs) (엣지 위치)는 인기 있는 콘텐츠가 시청자에게 빠르게 제공될 수 있음
- 클라우드 프론트 배포를 만들어야 함
- ELB는 캐싱이 불가
### Multi Site
- AWS Cloud를 활용하여 재해 복구 전략을 수립할 때 축소된 환경과 재해 발생 시 복구 시간 최소화
- 다중 사이트라 복구 시간이 최소화
### AWS Secrets Manager
- 데이터베이스 자격 증명을 자동으로 로테이션되는 경우?
- AWS Secrets Manager는 자동으로 키를 회전, 관리, 검색 가능
- KMS는 암호화 키를 쉽게 만들고 관리, 자격증명을 자동으로 갱신하는데는 불가
### S3
- 사내 데이터 센터에서 us-west-1 지역의 아마존 S3 버킷에 1페타바이트의 데이터를 복사 이 회사는 이제 us-east-1 지역의 다른 아마존 S3 버킷에 데이터의 일회성 복사본을 설치하기를 원하고 AWS 스노우볼을 사용할 수 없음
-  aws S3 sync 명령은 CopyObject APIs를 사용하여 Amazon S3 버킷 간에 개체를 복사
- S3 콘솔을 사용하여 다른 영역의 Amazon S3 버킷에 걸쳐 개체를 복사하도록 Amazon S3 배치 복제를 설정한 다음 복제 구성을 삭제
- Amazon S3 Batch Replication은 복제 구성이 실행되기 전에 존재했던 개체, 이전에 복제된 개체 및 복제에 실패한 개체를 복제
	- Batch Operations 작업을 통해 수행
### CloudFront distribution
- 어플리케이션 코드를 변경하지 않고 NLB 확장이 가능하도록
-  Amazon CloudFront Points of Presence (POPs) (엣지 위치)는 인기 있는 콘텐츠가 시청자에게 신속하게 제공될 수 있도록 함
- Leverage AWS Storage Gateway는 사내 어플리케이션을 클라우드 스토리지에 연결하고 지연시간을 줄여 캐싱
	- 최종 사용자에게 파일을 배포하는 데 사용할 수 없음 -> 사용자에게 노출 불가
- 트래픽이 급증할 때 콘텐츠가 역동적이고 자주 변경될 때 사용자에게 노출될 경우
### ELB  
- ELB가 대상 그룹을 unhealthy로 표시했지만 IP주소를 입력하면 엑세스 가능
- The route for the health check is misconfigured -> 상태 점검 경로가 잘못 구성됨
### Transit Gateway
- AWS 사이트 간 VPN 연결을 사용하고 있는데 AWS 클라우드에 대한 VPN 연결의 트래픽 급증으로 인해 사용자는 VPN 연결 속도가 느려지고 있을 때
- 다중 경로 라우팅을 사용해 Transit Gateway를 생성하고 추가 VPN 터널을 추가
- Transit Gateway는 여러 VPC 간 연결을 단순화 할 수 있고 단일 VPN 연결로 Transit Gateway에 연결된 모든 VPC에 연결 가능
###  Lambda
- 서버 없는 아키텍처로 마이그레이션, AWS Lambda를 이 아키텍처의 백본으로 사용할 때 고려해야 할 주요 사항에 중점
- 람다는 VPC에서 작동하므로 공용 인터넷 주소와 AWS API에 엑세스 가능
- 공용 서브넷에 있는 NAT GW를 이용한 경로가 필요함
### Geoproximity routing
- 지리적 영역의 크기를 동적으로 변경 가능항 route 53
- Geolocation routing은 지리 고정
### Trust policy
- IAM 서비스가 지원하는 유일한 리소스 기반 정책은?
- 신뢰 정책은 주요 엔티티가 역할을 맡을 수 있는지를 정의
- ID이면서 리소스 기반 정책
### Aurora Read Replicas
- 아마존 API 게이트웨이와 AWS 람다를 사용하여 서버리스 애플리케이션을 구축 아메리카에서 시작되었으며 이제 유럽으로 확장하여 지연 시간을 개선하기 위해 읽기 전용 버전을 사용할 수 있음 AWS 클라우드 포메이션을 사용하여 아마존 API 게이트웨이와 AWS 람다를 배포할 계획이지만 유럽에서도 데이터의 읽기 전용 복사본을 갖고 싶음
- 오로라 복제본은 오로라 DB 클러스터의 독립적인 끝점으로 읽기 작업을 확장하는 데 가장 적합
### Internet GW
- PC의 서브넷에 있는 인스턴스에 대해 인터넷에 액세스하거나 인터넷에서 액세스할 수 있도록 설정하려면
	1. VPC에 인터넷 게이트웨이를 연결
	2. 서브넷의 경로 테이블에 인터넷 인바운드 트래픽을 인터넷 게이트웨이로 유도하는 경로를 추가
	 3. 서브넷의 인스턴스에 글로벌하게 고유한 IP 주소가 있는지 확인
	 4. 네트워크 액세스 제어 목록 및 보안 그룹 규칙을 통해 관련 트래픽이 인스턴스로 오고 갈 수 있는지 확인
### Gateway Endpoint
- EC2 인스턴스를 사설 서브넷에 배포 WS 서비스에 안전하게 액세스할 수 있도록 VPC 엔드포인트를 사용하고 싶고, 게이트웨이 엔드포인트를 지원하는 두 개의 AWS 서비스는?
- DynamoDB, S3
- 인터페이스 엔드포인트
	- 지원되는 서비스로 향하는 트래픽의 진입점 역할을 하는 서브넷의 IP 주소 범위에서 개인 IP 주소를 갖는 ENI임
- 게이트웨이 엔드포인트
	- 지원되는 서비스로 향하는 트래픽에 대해 경호 테이블에거 경로의 대상으로 지정하는 게이트웨이
- 게이트웨이 엔드포인트는 AWS 네트워크를 통해 VPC에서 Amazon S3에 액세스하도록 경로 테이블에 지정하는 게이트웨이
- 인터페이스 엔드포인트는 VPC 피어링 또는 AWS Transit Gateway를 사용하여 VPC 내, 사내 또는 다른 AWS 지역의 VPC에서 Amazon S3로 요청을 라우팅하기 위해 개인 IP 주소를 사용하여 게이트웨이 엔드포인트의 기능
