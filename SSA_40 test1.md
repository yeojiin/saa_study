### AWS Global  Accelerator
- 지역이 다운될 경우 빠르게 글로벌로 복구가 가능(**글로벌 고정 IP 사용**)
-  cloudFront는 이미지나 비디오처럼 캐시 가능한 내용과 API 가속 및 동적 사이트 전달 같은 **동적 내용 모두에** 대해 성능을 향상 -> 엣지 로케이션으로부터 제공
 - Global Accelerator는 TCP나 UDP상의 다양한 애플리케이션 성능을 향상
 - 모든 요청이 애플리케이션 쪽으로 전달 -> 캐싱은 불가능
- 게임이나 IoT 또는 Voice over IP 같은 비 HTTP를 사용할 경우에 적합
- Global Accelerator는 TCP나 UDP상의 다양한 애플리케이션 성능을 향상
### Amazon DynamoDB Accelerator (DAX)
- 가용성이 높은 인메모리 캐시
- 개발자가 캐시 무효화, 데이터 수 또는 클러스터 관리를 하지 않고도 DynamoDB 테이블에 인메모리 가속화를 추가하는 데 필요한 모든 무거운 작업 가능
- 추가
	- DynamoDB는 NoSql
	- Aurora는 rds
### Amazon S3 Transfer Acceleration
- 클라이언트와  S3 버킷 간의 장거리 파일을 빠르고 안전하게 전송 가능
- S3TA는 클라우드프론트의 전세계 분산된 에지 위치를 활용
- **데이터가 엣지 위치에 도착하면 최적화된 네트워크 경로를 통해 S3로 라우팅**
### Auto Scaling
- `대상 추적 스케일링` 정책을 사용할 때 cloudwatch 경보를 생성하고 지표를 지정해 대상값을 유지할 수 있음
- simple scaling이낭 step scaling은 지표를 설정할 수 없음
### CloudTrail
- 불법, 보안 위협을 느끼는 시도가 감지될 경우 cloudTrail 로그 데이터를 수집하여 모니터링하고 모범 사례를 위한 거버넌스 프레임워크를 만들 수 있음
- 프로비저닝 없이 API활동을 모니터링하고 로그를 대규모로 분석하며 악의적인 활동이 발견되면 조치를 취할 수 있음
- 로그에 지표 필터를 생성하고 알람을 보낼 수 있음
- Kensis로 데이터를 스트리밍 불가 -> **cloudTrail은 S3버킷과 cloudWatch 로그만 가능**
### HPC 워크로드 ❗️
- HPC 는 노드 간 통신에 필요한 낮은 지연시간 네트워크 성능을 달성해야함
- 클러스터 배치 그룹은 가용성 영역 내에서 인스턴스를 가깝게 포장하기 때문에 **네트워크 지연시간이 짧거나 처리량이 높음**
### ECS❗️
- 완전 관리형 컨테이너 오케스트레이션 서비스
- ECS를 사용하면 AWS에서 도커 컨테이너 어플리케이션을 쉽게 실행, 확장, 보안할 수 있음
- **Fargate를** 사용하면 컨테이너화된 어플리케이션을 요청하는 **vCPU및 메모리 리소스의 양을** 지불하게 됨
- EC2 launch type을 이용하면 추가 요금 없이 EBS 볼륨 금액만 지불하게 됨
### Kinesis Data Stream ❓
- kinesis data firehose와 달리 **S3에 출력을 직접할 수 없음** -> 데이터 덤프를 위해 중개형 Lambda 기능을 통한 통합 기능을 제공하지 않음
- 따라서 사용자가 직접 Lambda 기능이 들어오는 스트림 처리 후 코딩을 해야 버퍼가 언정적으로 유지되고 데이터 손실이 일어나지 않음
- 스트리밍 데이터를 실시간으로 수집, 버퍼링 처리 가능 -> 빅데이터 등에 유리
### KMS 키
- kms 키 삭제는 대기 기간을 강제함 -> 키 삭제를 예약 해야하는데 7~30일 임
### Amazon RDS
- 클라우드에서 관계형 DB를 쉽게 설정, 운영, 확장 하는 관리형 서비스
- 자동으로 DB를 백업하고 소프트웨어를 최신으로 유지함
- RDS는 DB의 호스트 OS에 액세스하는 것을 허용하지 않음
- `RDS Custom for Oracle`을 사용하면 RDS에서 Oracle의 최소한 인프라 유지 보수 작업이 가능하도록 지원해줌
- 고가용성을 위해 multi-AZ 구성을 선택해야함
### S3 Standard IA
- 비정기적으로 접속하지만 신속한 엑세스가 필요할 때 
- 높은 내구성과 처리량, 낮은 지연시간과 가격
- 99.9% 가용성으로 데이터를 사용할 수 없는 경우 데이터가 성공적으로 검색될 때까지 작업이 자동으로 호출됨
### S3 version
- 개체의 여러 변형을 동일한 버킷에 유지하는 수단
- s3 버킷에 저장되는 모든 개채의 모든 버전을 보전, 검색, 복원할 수 있어서 versioning 지원 버킷으로 삭제하거나 덮었흔 개체를 복구할 수 있음
### FSx Luste
- `DFS`를 지원하는 데이터 집약적인 분석 워크로드 실행을 원할 때
- 윈도우 서버를 기반으로 구축되어 있으며, FSx는 마이크로소프트의 DFS를 사용하여 공유할 수 있음
- 고성능 파일 시스템을 쉽고, 비용 효율적으로 사용 가능
- 머신러닝, 고성능 컴퓨팅(HPC), 비디오 처리, 금융 모델링 등과 같은 워크로드에서 사용됨
- S3와 통합되어 있어 S3개체를 파일로 투명하게 표시하고 변경된 데이터를 S3에 다시 쓸 수 있음
- 데이터를 `병령 및 분산 방식`으로 처리, 빠르게 업로드하고 참고용으로 보관이 용이한 데이터 (S3)
### Reserved Instance 
- 온디멘드보다 가격이 저렴
- 계속해서 사용한다는 일정이 있으니 -> 예약
### VPN
- 구내에서 AWS전용 네트워크 연결을 쉽게 구축할 수 있음
- direct connect plus VPN을 사용하면 하나 이상의 AWS Direct Connect 전용 네트워크 연결을 VPC VPN과 결합할 수 있음
- IPsec 암호화 연결을 제공
### Route 53
- route 53 기반 지리적 위치 라우칭 정책을 사용하면 배포 권한이 있는 위치로만 콘텐츠 배포를 제한할 수 있음 -> 특정 지역으로만 배포가 가능하도록
- 사용자의 지리적 위치를 기반으로 트래픽을 처리하는 리소스를 선택할 수 있기 때문에 사용자의 국가를특정할 수 있음
### ❓38
- 람다에 대한 SNS 전달이 람다 계정 동시 할당을 초과 -> 지원팀 문의
### ElastiCache Memcache
- S3의 정적 콘텐츠를 제공하는 캐시로 사용할 수 없음
-  cloudFront에서 캐시 기능을 제공하는데 정적 컨텐츠도 제공 가능
### ❓❓❓51
