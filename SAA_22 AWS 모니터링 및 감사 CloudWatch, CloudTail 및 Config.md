### CloudWatch 지표 개요
- CloudWatch는 AWS의 모든 서비스에 대한 지표를 제공 -> 모니터링 가능
- 지표(Metric)는 모니터링할 변수
	- EC2 인스턴스의 지표:  CPUUtilization, NetworkIn 등
	- Amazon S3의 지표로는 버킷 크기 등
- 지표는 이름공간(namespaces)에 속하므로 각각 다른 이름공간에 저장되며, 서비스당 이름공간은 **하나**
- 지표의 속성으로 측정 기준(Dimension)이 있음
	- CPU 사용률에 대한 지표는 특정 인스턴스 ID나 특정 환경 등과 관련있음
- 지표당 최대 측정 기준은 30개
- 지표는 시간을 기반으로 하므로 `타임스탬프`가 꼭 있어야 하고, CloudWatch 대시보드에 추가해 볼 수 있음
- CloudWatch 사용자 지정 지표를 만들 수도 있음
	- 예를 들어 EC2 인스턴스로부터 메모리 사용량을 추출
- CloudWatch 외부로 스트리밍할 수 있음
- CloudWatch 지표를 원하는 대상으로 지속적으로 스트리밍하면 거의 실시간으로 전송되고 지연 시간도 짧아짐
	- Amazon Kinesis Data Firehose가 대상이 될 수 있음
	- Datadog, Dynatrace New Relic, Splunk, Sumo Logic 같은 타사 서비스 제공자로 CloudWatch 지표를 직접 전송할 수도 있음
	- 작동 원리
		- CloudWatch 지표는 Kinesis Data Firehose로 거의 실시간으로 스트리밍 -> 별도 설정 필요
		- Kinesis Data Firehose에서 Amazon S3 버킷으로 지표를 보내 Amazon Athena를 사용해 지표를 분석할 수 있음
		- Amazon Redshift를 사용해 지표로 데이터 웨어하우스를 만들거나 Amazon OpenSearch를 사용해 대시보드를 생성하고, 지표 분석을 할 수도 있음
		- 모든 이름공간에 속한 모든 지표를 스트리밍하거나 몇몇 이름공간의 지표만 필터링할 수 있음
		- 필터링한 지표의 서브셋만 Kinesis Data Firehose로 보낼 수 있음
- CloudWatch 지표의 장점은 기간을 클릭해 영역을 선택하면 해당 지표의 세부 데이터가 노출됨 -> 5분 단위 지표 생성
- 인스턴스에 세부 모니터링을 활성화하면 1분 단위의 데이터를 얻을 수 있음
- 데이터를 시간으로 필터링하고, 데이터를 누적 영역형이나 선 숫자, 파이형 차트로 볼 수도 있음
- 대시보드에 추가하거나 .csv 파일로 다운로드해 공유 가능
- 원하는 리전을 기반으로 혹은 측정 기준을 기반으로 지표를 한눈에 볼 수도 있고, 리소스 ID로 필터링할 수도 있음
### CloudWatch 로그 개요
- CloudWatch Logs는 AWS에서 애플리케이션 로그를 저장
- `로그 그룹`을 정의해야 함 -> 보통은 애플리케이션을 나타내는 이름으로
- 로그 그룹 안에 다수의 로그 스트림이 있음, 애플리케이션 안의 로그 인스턴스나 특정한 로그 파일 또는 클러스터의 일부로서 갖고 있는 특정한 컨테이너를 나타냄
- 로그 만료 정책을 정의 -> 만료 시간 정의
- CloudWatch Logs를 다양한 대상으로 전송 가능
	- 배치 형태로 Amazon S3에서 내보낼 수 있고요, 혹은 그걸 Kinesis Data Streams, Kinesis Data Firehose, AWS, 람다, Amazon OpenSearch에 스트리밍할 수도 있음
	- 기본값으로 모든 로그가 암호화되고, 원한다면 자신의 키를 사용해서 자체적으로 KMS 기반 암호화를 설정 가능
	- SDK를 사용해서 로그를 전송할 수 있고, CloudWatch Logs Agent나 CloudWatch Unified Agent를 사용할 수도 있음
	- CloudWatch Unified Agent가 로그를 CloudWatch에 전송 가능해 잘 사용하지 않음
	- Elastic Beanstalk을 사용해서 애플리케이션에서 로그를 수집하고 직접 CloudWatch로 보낼 수 있음
	- ECS는 로그를 컨테이너에서 곧바로 CloudWatch로 보냄
	- 람다는 함수 자체에서 로그를 전송
	- VPC Flow Logs는 VPC 메타데이터 네트워크 트래픽의 특정한 로그를 전송
	- API Gateway는 API Gateway로 오는 모든 요청을 CloudWatch Logs로 보냄
	- CloudTrail은 직접 필터에 기반한 로그를 전송
	- Route 53은 서비스에 대한 모든 DNS 쿼리를 로깅
-  CloudWatch Logs의 로그를 쿼리하려면?
	- CloudWatch Logs Insights를 이용할 수 있음 -> CloudWatch Logs 안의 쿼리 기능
	- 쿼리를 작성 가능
	- 쿼리에 적용하려는 시간대를 지정하고, 자동적으로 결과를  시각적으로 받게 됨
	- 시각화를 생성한 특정한 로그 라인을 볼 수도 있음
	- 시각화는 결과로서 내보내거나 대시보드에 추가해서 원할 때마다 다시 생성 가능
	- CloudWatch Logs 안에서 로그 데이터를 검색하고 분석 가능
	- CloudWatch Logs Insights 콘솔의 일부로서 많은 샘플 쿼리가 제공
	- 특별한 목적으로 제작된 쿼리 언어를 제공
	- 쿼리를 제작하도록 해주는 모든 필드를 자동으로 CloudWatch Logs가 자동으로 탐지
		- 집계 통계를 계산하고 이벤트를 정렬하거나 이벤트 개수를 제한하는 등의 작업 가능
	- 쿼리를 저장하고 그것들을 CloudWatch 대시보드에 추가 가능
	- 다른 계정에 있어도 한 번에 다수의 로그 그룹을 쿼리할 수도 있음
	- **CloudWatch Logs Insights는 실시간 엔진이 아니라 쿼리 엔진이라는 점**
		- 쿼리를 실행하면 과거 데이터만 쿼리하게 됨
	- CloudWatch Logs는 여러 대상으로 내보낼 수 있음
		- Amazon S3 -> 이건 모든 로그를 Amazon S3로 전송하는 배치 내보내기에 사용, 완료될 때까지 최장 12시간까지 걸릴 수 있음
		- 내보내기를 시작하기 위한 API 호출을 CreateExportTask
		- 배치 내보내기이기 때문에 실시간이나 근 실시간이 아님
		- CloudWatch Logs Subscription을 사용, 그러면 로그 이벤트들의 실시간 스트림을 얻을 수 있고, 그걸 처리하고 분석할 수 있음
		- 그 데이터를 Kinesis Data Streams, Kinesis Data Firehose 또는 람다 등의 다양한 곳에 전송할 수 있음
		- Subscription Filter를 써서 전송 대상으로 전달하려는 로그 이벤트의 종류를 지정할 수 있음
			- 즉 Subscription Filter는 데이터를 Kinesis Data Streams에 전송할 수 있음
		- 예를 들어 Kinesis Data Firehose, Kinesis Data Analytics 또는 Amazon EC2 또는 람다와의 통합을 이용하려고 할 경우 유용
		- 직접적으로 데이터를 Kinesis Data Firehose로 전송할 수 있음
		- 근 실시간으로 Amazon S3나 예를 들어 OpenSearch 서비스에 전송할 수 있음
		- 또는 커스텀 람다 함수를 작성하거나 실시간으로 OpenSearch 서비스로 데이터를 전송하는 관리형 람다 함수를 사용할 수도 있음 
		- 그 외에도 Subscription Filter로 다양한 계정과 리전에서 온 CloudWatch Logs 데이터를 하나의 특정 계정에서 Kinesis Data Streams 등에 집계할 수 있음
		- 그걸 Kinesis Data Firehose로 보내고 다시 근 실시간으로 Amazon S3로 보내게 됨
		- 이런 식으로 **로그 집계**를 할 수 있음
		- 반드시 **전송 대상을 사용**
			- 발신자 계정과 수신자 계정이 있다면 CloudWatch Logs Subscription Filter를 만들고, 수신자 계정에서 Kinesis Data Stream을 가상으로 표시하는 Subscription Destination으로 전송됨
			- 전송 대상 액세스 정책을 첨부해서 첫 번째 계정이 실제로 데이터를 전송 대상에 전송하도록 허용
				- 수신자 계정에서 레코드를 Kinesis Data Stream에 전송할 권한을 가진 IAM 역할을 생성
		- 이 역할을 첫 번째 계정이 맡을 수 있는지 확인
		- 모두 설정되면 CloudWatch Logs에서 나온 데이터를 한 계정에서 다른 계정의 전송 대상으로 전송할 수 있음
### CloudWatch Logs - Live Tail - Hands On
- Live Tail을 활성화 하면 CloudWatchLogs로 이벤트가 올라오면서 Live Tail에 바로 나타남
- 로그가 언제 일어난 것인지, 그룹이 어떤 것인지 등을 알 수 있음
- CloudWatch 로그를 쉽게 디버깅 할 수 있음
- 하루에 한 시간만 무료
### CloudWatch 에이전트 및 CloudWatch Logs 에이전트
- CloudWatch 에이전트를 이용해서 EC2 인스턴스에서 로그와 지표를 받아 CloudWatch에서 표시할 수 있는 방법?
- EC2 인스턴스에서 CloudWatch로는 기본적으로 어떤 로그도 옮겨지지 않음
- EC2 인스턴스에 에이전트라는 프로그램을 실행시켜 원하는 로그 파일을 푸시해야 함
- CloudWatch Logs 에이전트가 EC2 인스턴스에서 작동해 CloudWatch Logs로 로그를 보냄
- EC2 인스턴스에 로그를 보낼 수 있게 해주는 IAM 역할이 있어야 함
- 이 에이전트는 온프레미스 환경에서도 셋업될 수 있음
	- VM-ware 같은 가상 서버에서 온프레미스 환경으로 서비스를 제공할 수 있음
- 같은 에이전트를 설치하면 CloudWatch Logs로 로그를 보낼 수도 있음
- CloudWatch에는 두 가지 에이전트
	- 좀 더 오래된 CloudWatch Logs 에이전트
		- CloudWatch Logs로 로그만 보냄
	- 좀 더 최근에 나온 통합 CloudWatch 에이전트
		- 프로세스나 RAM 같은 추가적인 시스템 단계 지표를 수집 후 CloudWatch Logs에 로그를 보냄
		- 지표와 로그를 둘 다 사용해 통합 에이전트
		- SSM Parameter Store를 이용해서 에이전트를 쉽게 구성 가능
		- 모든 통합 에이전트를 대상으로 중앙 집중식 환경 구성을 할 수 있음
		- 로그를 보낼 수 있음
	- 둘 다 EC2 인스턴스나 온프레미스 서버 같은 가상 서버를 위한 것
- 에이전트를 EC2 인스턴스나 Linux 서버에 설치하면 지표를 수집할 수 있는데요 어떤 지표일까?
- 세분화된 수준으로 CPU 지표를 가져올 수 있음 
	- active, guest , idle, system user, steal 등
	- 디스크 지표는 free, use, total 
	- 디스크 IO에는 writes, reads, bytes, iops
	- RAM은 free, inactive, used, total, cached
	- 넷 상태에서는 TCP와 UDP 연결, 넷 패킷, 바이트 수 등
	- 프로세스에서 얻을 수 있는 지표는 total, dead, bloqued idle, running, sleep
	- 메모리처럼 쓰는 디스크인 스와프 공간에서는 free, used, used %를 볼 수 있음
- **통합 CloudWatch 에이전트가 기본 EC2 인스턴스 모니터링보다 더 세부적이고 많은 지표를 수집**
- EC2에서 곧장 디스크, CPU, 스와프나 메모리가 아닌 네트워크에 대한 지표를 얻을 수 있긴 하지만 상당히 포괄적인 지표만 얻을 수 있음
- 세부 지표를 얻고 싶다면 **통합 CloudWatch 에이전트**를 이용
### CloudWatch 경보 개요
- 메트릭에서 나오는 알림을 트리거하는 데 사용됨
- 샘플링, 퍼센티지, 최대, 최소 등등 다양한 옵션으로 복잡한 알람을 정의할 수 있음
- 알람에는 세 가지 상태가 있음
	- OK는 알람이 트리거되지 않았다는 의미
	- INSUFFICIENT DATA는 알람이 상태를 결정하기 위한 충분한 데이터가 없다는 의미
	- ALARM은 여러분의 한도 값을 초과했다는 의미, 그럼 알림이 전송됨
- 기간은 알람이 얼마나 오랫동안 메트릭을 평가할지를 나타냄
-  알람 주요 타깃 세 가지
	- 정지, 종료, 재부팅 또는 복구 등 EC2 인스턴스에 대한 **액션**
	- 오토 스케일링 액션을 트리거
		- 예를 들면 스케일 아웃이나 스케일 인 등
	- SNS 서비스에 알림을 전송
		- 예를 들어 SNS 서비스 람다 함수에 연결해서 어떤 알람인지에 따라서 사실상 원하는 건 뭐든지 할 수 있음
- 복합 알람
	- CloudWatch Alarms는 **하나의 메트릭**에 적용되기 때문
	- 하지만 다수의 메트릭을 사용하길 원한다면 여러분은 복합 알람을 사용
	- 복합 알람은 실제로 다수의 다른 알람들의 상태를 모니터링하고, 그런 알람들은 각각 하나의 다른 메트릭에 의존할 수 있기 때문
	- 복합 알람은 그런 모든 다른 알람들을 결합하는 것
	- AND 또는 OR 조건을 사용해서 여러분이 검사하는 조건을 아주 유연하게 다룰 수 있음
	- 알람 노이즈를 줄이는 데 아주 유용
	- 복잡한 복합 알람을 만들 수 있기 때문
		- 만일 CPU가 High이고 네트워크가 High라면 알람을 하지 말라고 할 수 있음 -> CPU가 High이고 네트워크가 Low일 때만 알려고 하기 때문
		- EC2 인스턴스가 있고, 우린 별도로 복합 알람을 만들 것
		- 첫 번째 기본 알람, 즉 알람 A를 만들고요, 그게 EC2 인스턴스의 CPU를 모니터링
		- 다음으로 여러분은 알람 B를 만듦, EC2 인스턴스의 IOPS를 모니터링
		- 복합 알람은 알람 A와 알람 B의 결합으로 정의함
		- 만일 알람 A가 알람 상태이고 알람 B가 알람 상태라면 복합 알람이 알람 상태가 됨
		- SNS 알림을 트리거할 수 있음
- EC2 인스턴스 복구
	- 상태 검사가 있고 EC2 VM 검사가 있음
	- 기본 하드웨어를 검사하는 시스템 상태 검사도 있음
	- 그 두 가지 검사에 CloudWatch Alarms을 정의할 수 있음
	- 특정한 EC2 인스턴스를 모니터링하게 됨, 그리고 만일 알람이 발생하면 EC2 인스턴스 복구를 시작해서 가령 EC2 인스턴스를 한 호스트에서 다른 호스트로 옮길 수 있음
	- 복구를 할 때는 여러분의 인스턴스에 대해 동일한 프라이빗 IP, 퍼블릭 IP, 탄력적 IP를 얻게 되고요, 동일한 메타데이터, 동일한 배치 그룹을 얻게 됨
	- 알람을 SNS 토픽에 전송해서 EC2 인스턴스가 복구되었다는 걸 알 수 있음
- CloudWatch Logs 메트릭 필터를 기초로 알람을 생성할 수 있음
- CloudWatch Logs는 메트릭 필터가 있고, 그게 CloudWatch Alarms와 연결됨
- 특정한 단어, 예를 들면 error 같은 단어를 너무 많이 받은 경우에 경보를 발하고 메시지를 Amazon SNS에 전송할 수 있음
- 만일 알람 알림을 테스트하고 싶으면 여러분은 set-alarm-state라는 CLI 호출을 사용할 수 있음
- 특정한 한도 값에 도달하지 않았지만 알람을 트리거하고 싶을 때 유용
- 트리거된 알람이 여러분의 인프라를 위해 적절한 액션을 유발하는지 확인하고 싶기 때문
### EventBridge 개요 (구. CloudWatch Events)
- Amazon EventBridge: AWS 서비스의 예전 이름은 CloudWatch Events
- Amazon EventBridge의 기능은 다양
- 클라우드에서 CRON 작업을 예약할 수 있음 -> 스크립트를 예약할 수 있음
	- 한 시간마다 Lambda 함수를 트리거해서 스크립트를 실행
-  이벤트 패턴에 반응할 수도 있음
- 특정 작업을 수행하는 서비스에 반응하는 이벤트 규칙이 있음
	- 예를 들어 콘솔의 IAM 루트 사용자 로그인 이벤트에 반응할 수 있음
	-  이벤트가 발생하면 SNS 주제로 메시지를 보내고, 이메일 알림을 받을 수 있음
	- 누군가 루트 계정을 사용하면 이메일을 받을 수 있으니 계정 보안 기능으로 유용
- 대상(destination)이 다양하다면 Lambda 함수를 트리거해서 SQS, SNS 메시지 등을 보낼 수 있음
- Amazon EventBridge가 중앙에 위치하고, Amazon EventBridge로 보낼 다양한 소스가 있음
- 시작, 중단, 종료할 때는 EC2 Instance에서 실패한 빌드는 CodeBuild에서 객체 업로드 이벤트가 발생할 때는 S3 Event에서 계정의 새 보안 문제는 Trusted Advisor에서 가져옴
- EventBridge와 CloudTrail을 결합하는 건 좋은 조합
- AWS 계정에서 생성된 API 호출을 모두 가로채므로 아주 유용
- 일정 및 CRON 작업도 예약할 수 있음
- Amazon EventBridge로 전송되는 이벤트에 필터를 설정 가능
	- 예를 들어 Amazon S3에 있는 특정 버킷의 이벤트만 필터링하도록 하면 Amazon EventBridge는 이벤트의 세부 사항을 나타내는 JSON 문서를 생성
	- 어떤 인스턴스가 이 ID로 실행되는지 등을 나타내고, 시간, IP 등의 정보가 담김
	- JSON 문서 생성이 끝나면 이벤트들을 다양한 대상으로 전송할 수 있고, 유용한 통합 기능을 사용할 수 있음
	- Lambda 함수를 트리거하는 일정을 예약하거나 AWS Batch에서 배치 작업 예약을 하거나 Amazon ECS에서 ECS 작업을 실행할 수 있음
	- SQS, SNS나 Kinesis Data Streams로 메시지를 보낼 수도 있음
	- 단계 함수를 실행할 수도 있고, CodePipeline으로 CI/CD 파이프라인을 시작하거나 CodeBuild에서 빌드를 실행할 수 있음
	- SSM 자동화를 실행하거나 EC2 가동, 중지, 재개와 같은 특정 EC2 작업을 실행할 수도 있음
-  Amazon EventBridge는 기본 이벤트 버스(Default Event Bus)
	- 이벤트를 기본 이벤트 버스로 전송하는 AWS의 서비스를 말함
- 파트너 이벤트 버스(Partner Event Bus)라는 서비스가 있음
	- 파트너와 통합된 AWS 서비스를 말하는데 대부분의 파트너는 소프트웨어 서비스 제공자
	- 파트너들은 파트너 이벤트 버스로 이벤트를 전송
	- Zendesk 혹은 Datadog, Auth0 등을 사용할 수 있고 파트너 목록이 있음
	- 특정 파트너 이벤트 버스로 이벤트를 전송할 수 있고, 계정을 통해 AWS 외부에서 일어나는 변화에 반응할 수 있음
- 사용자 지정 이벤트(Custom Event Bus) 버스 -> 자체 버스를 만들 수 있음
	- 애플리케이션의 자체 이벤트를 사용자 지정 이벤트 버스로 전송
	- Amazon EventBridge 규칙 덕분에 다른 이벤트 버스처럼 여러 대상으로 이벤트를 보낼 수 있음
	- 리소스 기반 정책을 사용해 다른 계정의 이벤트 버스에 액세스할 수도 있음
	- 이벤트 아카이빙도 가능
	- 전부 또는 필터링된 서브셋을 아카이빙할 수 있고, 이벤트를 아카이빙할 때는 보존 기간을 무기한이나 일정 기간으로 설정할 수 있음
	- 이를 통해 아카이브된 이벤트를 재생할 수 있음
		- 예를 들어 Lambda 함수의 버그를 수정할 때 수정 후에 이벤트를 다시 테스트하고 재생해 함
		- 이때 아카이브된 이벤트를 재생하면 됨
		- 디버깅에 아주 유용
		- 트러블 슈팅이나 프로덕션을 수정할 때도 유용
-  EventBridge는 여러 곳에서 이벤트를 받음
- 이벤트가 어떻게 생겼는지 파악해야 함
- 이벤트는 JSON 형식
- 스키마 레지스트리(schema registry)라는 것이 있는데 Amazon EventBridge는 버스의 이벤트를 분석하고 스키마를 추론하는 능력이 있음
	- 스키마 레지스트리의 스키마를 사용하면 애플리케이션의 코드 생성할 수 있고, 이벤트 버스의 데이터가 어떻게 정형화되는지 미리 알 수 있음
	- 특정 코드 파이프라인 작업의 스키마를 예시
		- 코드를 다운로드하면 EventBridge가 이벤트 버스로부터 스키마 추론 방법과 데이터의 정형화 방식을 파악함
		- 스키마를 버저닝할 수도 있어 애플리케이션의 스키마를 반복할 수 있음
- EventBridge의 리소스 기반 정책(resource-based policy)
	- 특정 이벤트 버스의 권한을 관리할 수 있음
		- 예를 들면 특정 이벤트 버스가 다른 리전이나 다른 계정의 이벤트를 허용하거나 거부하도록 설정
		- 리소스 기반 정책의 사용 사례
			- 여러 계정의 모음인 AWS Organizations의 중앙에 이벤트 버스를 두고 모든 이벤트를 모음
			- central-event-bus 계정에 다른 계정이 이벤트를 보낼 수 있도록 리소스 기반 정책을 추가하면 오른쪽에 있는 다른 계정에서 PutEvents 작업을 통해 이벤트를 central-event-bus로 전송할 수 있음
- 기본 이벤트 버스와 파트너 이벤트 버스 덕분에 계정에서 발생하는 이벤트에 반응할 수 있고, 사용자 지정 이벤트 버스로 자체 이벤트에 반응할 수 있음
- 스키마 레지스트리 능력이 있고, 리소스 기반 정책을 활용하면 다른 계정의 이벤트를 수신하거나 거부하도록 설정할 수 있음
### CloudWatch Insights and Operational Visibility
-  **CloudWatch Container Insights** :  컨테이너로부터 지표와 로그를 수집, 집계, 요약하는 서비스
	- Amazon ECS나 Amazon EKS EC2의 Kubernetes 플랫폼에 직접 실행하는 컨테이너에서 사용할 수 있음
		- 컨테이너화된 버전의 CloudWatch 에이전트를 사용해야 컨테이너를 찾을 수 있음
	- ECS와 EKS의 Fargate에 배포된 컨테이너에서도 사용할 수 있음
	- 컨테이너로부터 지표와 로그를 손쉽게 추출해서 CloudWatch에 세분화된 대시보드를 만들 수 있음
- **Lambda Insights** : AWS Lambda에서 실행되는 서버리스 애플리케이션을 위한 모니터링과 트러블 슈팅 솔루션
	- CPU 시간, 메모리 디스크, 네트워크 콜드 스타트나 Lambda 작업자 종료와 같은 정보를 포함한 시스템 수준의 지표를 수집, 집계, 요약
	- Lambda 함수를 위해 Lambda 계층으로 제공
	- Lambda 함수 옆에서 실행되며 Lambda Insights라는 대시보드를 생성해 Lambda 함수의 성능을 모니터링한다는 것
	- AWS Lambda에서 실행되는 서버리스 애플리케이션의 세부 모니터링이 필요할 때 사용
- **Contributor Insights**: 기고자(Contributor) 데이터를 표시하는 시계열 데이터를 생성하고, 로그를 분석하는 서비스
	- 예를 들어 상위 N개의 기고자나 총 고유 기고자 수 및 사용량을 볼 수 있음
	- 서비스는 네트워크의 상위 대화자를 찾고, 시스템 성능에 영향을 미치는 대상을 파악할 수있음
	- VPC 로그, DNS 로그 등 AWS가 생성한 모든 로그에서 작동
	- 불량 호스트를 식별해 낼 수 있음
	- 네트워크 로그나 VPC 로그를 확인해서 사용량이 가장 많은 네트워크 사용자를 찾을 수 있음
	- DNS 로그에서는 오류를 가장 많이 생성하는 URL을 찾을 수 있음
	- 사용량이 많은 네트워크 사용자를 식별하는 방법?
		- 모든 네트워크 요청에 대해 VPC 내에서 생성되는 로그인 VPC 플로우 로그가 CloudWatch Logs로 전달되고, CloudWatch Contributor Insights가 분석
		- 이를 통해 VPC에 트래픽을 생성하는 상위 10개의 IP 주소를 찾고, 좋은 사용자인지 나쁜 사용자인지 판단
		- 상위 10개의 기고자 또는 비슷한 걸 본다면 Contributor Insights가 정답
		- **로그를 통해 상위 대상을 찾을 수 있음**
	- 규칙은 직접 생성하거나 AWS가 생성한 간단한 규칙을 활용할 수 있음
	- 백그라운드에서는 CloudWatch Logs가 활용됨
	- 내장된 규칙이 있어 다른 AWS 서비스에서 가져온 지표도 분석할 수 있음
- **CloudWatch Application Insights**: 모니터링하는 애플리케이션의 잠재적인 문제와 진행 중인 문제를 분리할 수 있도록 자동화된 대시보드를 제공
	- Java나 .NET Microsoft IIS 웹 서버나 특정 데이터베이스를 선택해 선택한 기술로만 애플리케이션을 실행할 수 있음
	- EBS, RDC, ELB ASG, Lambda, SQS, DynamoDB, S3 버킷과 같은 AWS 리소스에 연결
	- ECS 클러스터 EKS 클러스터와 SNS 주제 API Gateway도 있음
	- 애플리케이션에 문제가 있는 경우 CloudWatch Application Insights는 자동으로 대시보드를 생성하여 서비스의 잠재적인 문제를 보여 줌
	- 자동화된 대시보드를 생성할 때 백그라운드에서는 내부에서 SageMaker 머신 러닝 서비스가 사용됨 ->  애플리케이션 상태 가시성을 더 높일 수 있음
	- 트러블 슈팅이나 애플리케이션을 보수하는 시간이 줄어듦
	- AWS의 다른 리소스나 서비스를 사용하는 애플리케이션에 문제가 발생하면 자동으로 CloudWatch Applications Insights 대시보드에 표시해 줌
	- 발견된 문제와 알림은 모두 Amazon EventBridge와 SSM OpsCenter로 전달
	- 애플리케이션에서 문제가 탐지되면 여러분에게 알림이 감
- CloudWatch Container Insights는 ECS, EKS, EC2의 Kubernetes, Fargate로부터 오는 로그와 지표를 위한 것이고, Kubernetes를 사용한다면 `실행할 에이전트가 필요`
- CloudWatch Lambda Insights는 AWS Lambda에서 실행되는 서버리스 애플리케이션의 트러블 슈팅을 위한 세부 지표를 제공
- Contributors Insights는 CloudWatch Logs를 통해 상위 N개의 기고자를 찾음
- CloudWatch Application Insights는 자동화된 대시보드를 생성해 사용하는 애플리케이션과 관련된 AWS 서비스나 애플리케이션의 트러블 슈팅을 도움
### CloudTrail 개요 강의
- CloudTrail은 AWS 계정의 거버넌스, 컴플라이언스, 감사를 실현하는 방법
- CloudTrail은 기본값으로서 활성화
- CloudTrail을 사용하면 여러분의 AWS 계정 안에서 일어난 모든 이벤트와 API 호출 이력을 콘솔, SDK, CLI 또는 기타 AWS 서비스를 통해 얻을 수 있고 모든 로그가 CloudTrail에 나타남
- 그런 로그들을 CloudTrail에서 CloudWatch Logs나 Amazon S3에 넣을 수도 있음
	- 모든 리전에 걸쳐 누적된 이벤트 이력을 가령 하나의 특정한 S3 버킷에 넣고 싶으면 트레일을 생성해서 모든 리전이나 하나의 리전에 적용할 수 있음
- CloudTrail을 사용할 때 예를 들면 우리는 누군가가 가서 AWS의 뭔가를 삭제했다고 생각함
	- 가령 EC2 인스턴스가 종료되었을 때 누가 그렇게 했는지 알기 위해 CloudTrail을 확인->  CloudTrail 안에 그 API 호출이 남아 있어 누가 무엇을 했는지 파악 가능
- CloudTrail이 중간에 있고 SDK, CLI 또는 콘솔 또는 심지어 IAM 사용자와 역할 또는 기타 서비스의 액션들이 CloudTrail 콘솔 안에 있음
	- 검사하고 감사할 수 있음
- 모든 이벤트를 90일 이상 보관하고 싶으면 그걸 CloudWatch Logs나 S3 버킷에 전송할 수 있음
- CloudTrail에서 볼 수 있는 이벤트는 세 가지
- 관리 이벤트: AWS 계정 안에서 리소스에 대해 수행된 작업을 나타냄
	- 예를 들어 누군가가 보안을 설정할 때마다 그들은 IAM AttachRolePolicy라는 API 호출을 사용
	- CloudTrail에 나타남
	- 리소스나 AWS 계정을 수정하는 모든 게 CloudTrail에 나타날 거예요, 그리고 기본값으로서 트레일은 모든 관리 이벤트를 로깅하도록 설정
	- 리소스를 변경하지 않는 읽기 이벤트
		- 예를 들어 누군가가 IAM에서 모든 사용자를 나열하거나 EC2에서 모든 EC 인스턴스를 나열하는 이벤트
		- 리소스를 수정할 수 있는 쓰기 이벤트와 구분할 수 있음
		- 예를 들어 누군가가 DynamoDB 테이블을 삭제하거나 삭제 시도를 할 수 있음
		- 읽기 이벤트는 그냥 정보를 받는 것
- 데이터 이벤트: 
	- 고용량 작업이라 기본값으로서 데이터 이벤트는 로깅되지 않음, 선택사항임
	-  예를 들어 Amazon S3 객체 수준 활동 등의 GetObject나 DeleteObject, PutObject 등
	- S3 버킷에 대해 아주 많이 이루어질 수 있음
	- 기본값으로서 데이터 이벤트는 로깅되지 않음
	- 읽기 이벤트와 쓰기 이벤트를 분리할 수도 있음
	- 읽기 이벤트는 GetObject고요, 반면에 쓰기 이벤트는 DeleteObject나 PutObject
	-  AWS 람다 함수 실행 활동
		- 누군가가 Invoke API를 사용하면 람다 함수가 몇 번이나 호출되는지 알 수 있음
		- 람다 함수가 많이 실행된다면 용량이 아주 클 수 있음
- CloudTrail Insights
	- 모든 유형의 서비스에 걸쳐 아주 많은 관리 이벤트가 일어나고 계정에서 아주 많은 API 호출이 아주 빠르게 이루어진다면 어떤 게 이상한지, 비정상적인지 어떤 게 정상인지 알기가 상당히 어려움
	- CloudTrail Insights를 사용
	- CloudTrail Insights를 사용하려면 활성화를 하고 비용을 지불해야 함, CloudTrail Insights는 계정에서 일어나는 이벤트를 분석하고 비정상적인 활동에 대한 탐지를 시도
		- 예를 들어 부정확한 리소스 프로비저닝, 서비스 한도 도달, AWS IAM 액션의 폭증, 주기적 유지보수 활동 누락 등
	- CloudTrail은 정상적인 관리 활동을 분석해서 기준선을 만들고, 올바른 형태의 이벤트인지 모든 걸 계속적으로 분석
	- 뭔가 변경되거나 변경이 시도되면 비정상적인 패턴인지 탐지
	- 관리 이벤트는 CloudTrail Insights에 의해 계속적으로 분석
	- 탐지되면 인사이트 이벤트가 생성
	- 비정상적인 인사이트 이벤트가 CloudTrail 콘솔에 나타남
	- Amazon S3, EventBridge 이벤트로 전송 가능
	- CloudTrail Insights 이외에 자동화가 필요하면 CloudWatch 이벤트가 생성
		- 이메일을 전송할 수 있음
- CloudTrail 이벤트 보관
	- 기본값으로서 이벤트는 CloudTrail에 90일 동안 보관, 그 기간이 지나면 삭제
	- 감사를 목적으로 가령 1년 전에 일어난 뭔가를 다시 찾아보려고 할 경우엔 이벤트를 더 오래 보관하려 할 수 있음
		-  그 기간 이후에 이벤트를 보관하려면 여러분은 그걸 S3에 전송하고, Athena를 사용해서 그것들을 분석해야 함
		- 여러분의 모든 관리 이벤트, 데이터 이벤트, 인사이트 이벤트는 CloudTrail로 가서 90일 동안 보관되고, 그 이후에 장기간 보관하기 위해 여러분은 그것들을 S3 버킷에 로깅
		- 그것들을 분석할 준비가 되면 여러분은 S3의 데이터를 쿼리하는 서버리스 서비스인 Athena 서비스를 사용해서 여러분이 원하는 이벤트를 검색하고 분석
### CloudTrail - EventBridge 통합
- API 호출을 가로채기 위한 Amazon EventBridge와의 통합: **CloudTrail 통합**
- 사용자가 DeleteTable API 호출을 사용해서 DynamoDB에서 테이블을 삭제할 경우에 언제나 SNS 알림을 받고 싶을 경우
- AWS에서는 API 호출을 할 때마다 API 호출 자체가 CloudTrail에 로깅
- 모든 API 호출이 로깅
- 하지만 그런 모든 API 호출이 결국 이벤트로서 Amazon EventBridge에 남게 됨, 그럼 우린 그 아주 구체적인 DeleteTable API 호출을 확인할 수 있음 그걸로 규칙을 생성할 수 있음
- 그 규칙에는 대상이 있고,  대상은 Amazon SNS가 될 거예요, 그럼 우린 경보를 생성할 수 있음
- Amazon EventBridge와 CloudTrail을 통합하는 방법의 예
	- 어떤 사용자가 여러분의 계정에서 역할을 맡을 때마다 알림을 받길 원함
	- AssumeRole은 IAM 서비스에 있는 API, 그러므로 CloudTrail에 로깅됨
	- EventBridge 통합을 이용해서 SNS 토픽으로 가는 메시지를 트리거
	- 보안 그룹 인바운드 규칙을 변경하는 API 호출을 가로챌 수도 있음
	- 보안 그룹 호출은 AuthorizedSecurityGroupIngress라고 부름, 그건 EC2 API 호출
	- CloudTrail에 로깅됨, 그런 다음에 EventBridge에 나타남
	- SNS 알림을 트리거할 수 있음
### AWS Config 개요
- Config는 AWS 내 리소스에 대한 감사와 규정 준수 여부를 기록할 수 있게 해주는 서비스
- 설정된 규칙에 기반해 구성과 구성의 시간에 따른 변화를 기록할 수 있으며 이를 통해 필요할 경우 인프라를 빠르게 롤백하고 문제점을 찾아낼 수 있음
- Config로 해결할 수 있는 질문?
	- 보안 그룹에 제한되지 않은 SSH 접근이 있나'
	- 버킷에 공용 액세스가 있나?
	- 시간이 지나며 변화한 ALB 구성이 있나?
	- 규칙이 규정을 준수하든 아니든 변화가 생길 때마다 SNS 알림을 받을 수 있음
- Config는 리전별 서비스이기 때문에 모든 리전별로 구성해야 하며 데이터를 중앙화하기 위해 리전과 계정 간 데이터를 통합할 수 있음
- 모든 리소스의 구성을 S3에 저장해 나중에 분석할 수도 있음
	- Athena 같은 서버리스 쿼리 엔진을 통해서
- Config에는 어떤 규칙이 적용될까?
	- AWS 관리형 Config 규칙 -> 75종의 규칙이 있고, 커스텀 가능
	- AWS Lambda를 이용해 규칙 정의 가능
	- 각 EBS 디스크가 gp2 유형인지 평가할 수 있고, 개발 계정의 EC2 인스턴스가 t2.micro 유형인지 평가할 수도 있음
	- 몇몇 규칙들은 구성이 변화할 때마다 평가되거나 트리거 됨
	- 예를 들어 EBS 디스크에 새 구성이 생길 때마다 EBS 디스크의 유형을 평가를 하거나 정기적으로 규칙이 평가되도록 만들 수도 있음
	- 예를 들어 2시간마다 EBS 디스크가 gp2 유형인지 평가
- Config 규칙은 규정 준수를 위한 것 -> 어떤 동작을 미리 예방하지는 못함
- IAM 같은 보안 메커니즘을 대신할 수 없음
- 하지만 구성의 개요와 리소스의 규정 준수 여부는 Config 규칙을 통해 알 수 있음
- Config는 결제해야 하며 굉장히 비싸질 수 있음
- 보안 그룹은 규정 미준수 상태
- 리소스 구성도 시간별로 볼 수 있음 -> 언제, 누가 변경했는지 등
- 이걸 CloudTrail과 연결해 리소스에 대한 API 호출을 볼 수 있음
- 일어나는 모든 일을 전부 파악할 수 있음
- Config 내에서 행동을 차단할 수는 없지만 SSM 자동화 문서를 이용해서 규정을 준수하지 않는 리소스를 수정할 수 있음
	- 예를 들어 IAM 액세스 키의 만료 여부를 모니터링한다고 할 때
	- 키가 90일 이상 보관된 경우로 이런 경우는 규정을 준수하지 않은 상태
	- 규정 미준수를 예방하지는 못하지만 리소스가 규정을 미준수할 때마다 수정 작업을 트리거 할 수 있음
- RevokeUnusedIAM UserCredentials 라는 이름의 SSM 문서가 있고, 이걸 이용한다고 가정 그리고 이게 리소스에 적용된다고 할 때
	- 이 경우 SSM 문서가 IAM 액세스 키를 비활성화함 
- AWS 관리형 문서를 사용하든 본인만의 자동화 문서를 만들어 사용하든 규정을 준수하지 않는 리소스를 수정할 수 있음
- 만약 계속 스크립트를 하고 싶다면 람다 함수를 실행하는 문서를 생성해서 원하는 작업을 수행할 수도 있음
	- 수정 작업은 재시도 될 수 있음
	- 리소스를 자동 수정했음에도 여전히 규정을 미준수한다면 5번까지 재시도될 수 있음
- 알림
	- EventBridge를 사용해 리소스가 규정을 미준수했을 때마다 알림을 보낼 수 있음
	- 예를 들어 보안 그룹을 모니터링하다가 규정 미준수 상태가 됐다면 EventBridge에서 이벤트를 트리거 해서 원하는 리소스에 넘길 수 있음
	- 또는 모든 구성 변경과 모든 리소스의 규정 준수 여부 알림을 Config에서 SNS로 보낼 수도 있음
	- 한 구성 항목이 있고, 특정 이벤트에만 적용되게 필터링 하고 싶다면 SNS 필터링을 사용해 SNS 주제를 필터링하여 이메일 등으로 알림 전송 가능
	- 모든 알림을 한 곳에 집중시킴
### CloudTrail vs CloudWatch vs Config
- CloudWatch, CloudTrail Config를 각각 구분하는 것
- CloudWatch
	- 지표, CPU 네트워크 등의 성능 모니터링과 대시보드를 만드는 데 사용
	- 이벤트와 알림을 받을 수도 있고, 로그 집계 및 분석 도구도 사용할 수 있음
- CloudTrail
	- 계정 내에서 만든 API에 대한 모든 호출을 기록
	- 특정 리소에 대한 추적도 정의할 수 있음 -> EC2에 대한 정보만 더 얻을 수도 있음
	- 글로벌 서비스
- Config
	- 구성 변경을 기록하고, 규정 준수 규칙에 따라 리소스를 평가
	- 변경과 규정 준수에 대한 타임라인을 UI로 표시
- ELB에서 일어난 일을 이해하는 데 세 서비스가 어떻게 도움을 주는지 일래스틱 로드 밸런서를 통해 알아보면?
- CloudWatch는 들어오는 연결 수를 모니터링하고, 오류 코드 수를 시간 흐름에 따라 비율로 시각화할 수 있으며 로드 밸런서의 성능을 볼 수 있는 대시보드도 만들 수 있음
- 글로벌 애플리케이션에 로드 밸런서가 여러 개 있다면 글로벌 대시보드를 만들 수도 있음
- ELB에 왜 Config를 사용할까?
	- 로드 밸런서에 대한 보안 그룹 규칙을 추적해 누구도 수상하게 행동하거나 무언가 변경하지 못하게 하려고 사용
	- 혹은 누군가 SSL 인증서 등을 수정하지는 않는지 감시하기 위해 로드 밸런서의 구성 변경을 추적하는 데 사용할 수도 있음
	- 또한 SSL 인증서가 로드 밸런서에 항상 할당되어 있어야 한다는 규칙을 만들어 암호화되지 않은 트래픽이 로드 밸런서에 접근하지 못하게 할 수도 있음
	- Config에 두 가지 규칙을 적용 필요
		- CloudTrail은 누가 API를 호출하여 로드 밸런서를 변경했는지 추적
		- 누군가 보안 그룹 규칙을 바꾸거나 혹은 SSL 인증서를 바꾸거나 삭제한다면 CloudTrail이 누가 그랬는지 확인 가능
- 이 서비스들은 상호 보완적
