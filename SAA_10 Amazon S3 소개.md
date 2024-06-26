### S3 개요 
- 무한한 스케일링
- 용도
	- 백업과 스토리지 용
	- 재해 복구 용 -> 데이터를 백업하여 활용
	- 아카이브 용
	- 하이브리드 클라우드 
	- 어플리케이션 호스팅
	- 미디어 호스팅
	- 다량의 데이터 저장 및 데이터 분석
	- 소프트웨어 전달
	- 정적 웹사이트 호스팅
- S3는 파일을 버킷에 저장하는데 버킷은 상위 레벨 디렉토리로 표시되고 버킷의 파일은 객체라고 함
- 버킷은 <u>계정 안에 생성되고 전역적으로 고유한 하나의 이름이어야 함</u>
- 버킷은 <u>리전 수준</u>에서 정의됨
- 대문자나 밑줄이 없고, 3~63, 소문자나 문자로 시작, 아이피는 사용 불가, 접두가사 있음
- <u>S3 키는 파일의 전체 경로</u>
	- 접두사(prefix) + 객체 이름
		- s3://my-bucket/my_folder/another_folder/my_file.txt  -> 예시 키
- 객체
	- 최대 객체 크기는 5TB
	- 최대 용량보다 크다면 멀티파트로 여러개 나눠서 저장해야함
	- 메타 데이터도 있음 -> 객체의 key-value pairs
	- 태그는 10개까지 가능
### 보안
- 사용자 기반: IAM 정책 필요
- 리소스 기반: 
	- Bucket Policies: S3 콘솔에 직접 할당하는 전체 버킷 규칙 -> S3 버킷에 액세스 할 수 있는 교차 계정(cross account)
	- Object Access Control List(ACL)
	- Bucket Access Control List(ACL)
- IAM 원칙이 특정 API 호출 시 S3 객체에 액세스 할 수 있음
- 암호화
- 버킷 정책
	-  json으로 작성됨
	- Resource:  적용되는 버킷과 객체를 의미
		- arn:aws::s3:::examplebucket/*
	- Effect: allow/deny
	- Action: api 대상
	- Principal: 계정, 사용자 정책
		- * 이면 모든 사용자가 접근 가능 
### S3 웹 사이트
- html이나 이미지를 업로드 후 S3 버킷 주소를 제공하면 url로 웹사이트처럼 이용 가능
- `공개 읽기를 활성화`해야 웹사이트로 사용했을 때 사용자들이 아무나 읽을 수 있음
### 버킷 버전
- 버전으로 관리
- 삭제하면 진짜 삭제되는게 아닌 삭제 마크를 붙이는 거라 복구가 가능함
- 버전 관리 활성화 전에는 null 버전으로 관리됨
### S3 복제
- 어떤 버킷을 다른 버킷으로 복제할 떄
	- CRR: cross region replication 다른 리전으로 복사 -> 컴플라이언스(compliance) 즉 법규나 내부 체제 관리 그리고 데이터가 다른 리전에 있어 발생할 수 있는 지연 시간을 줄일 경우 사용, 계정 간 복제 시
	- SRR: same region replication 같은 리전으로 복사 -> 다수의 S3 버킷간의 로그 통합, 개발 환경이 별도로 있어 운영 환경과 개발 환경 간의 실시간 복제를 필요로 할 때
- 버킷 간 비동기 복제가 필요할 경우
	- 소스 버킷과 복제 대상 버킷 둘 모두 버전 관리 기능이 활성화 되어야 함
	- IAM 읽기, 쓰기 권한을 S3에 부여해야 함
- 복제 활성화 후 새로운 객체에 대해서만 복사가 가능함 - 동기로
- 기존 객체를 복제하려면 `S3 배치 복제 기능`을 사용해야 함
- 소스 버킷에서 대상 버킷으로 삭제 마커를 복제하면 됨
- 버전 ID로 삭제할 경우 영구 삭제 -> 복제할 때 버전ID는 복사되지 않음
- 버킷 1을 2로, 2를 3으로 복제 한다고 해서 1의 버킷 객체가 3번 버킷으로 복제되지는 않음, chaining 불가
### 스토리지 클래스
- 종류
	- standard
		- 가용성이 99.99%
		- 자주 액세스하는 데이터에 사용 됨
		- 지연시간이 짧고 처리량이 높음
		- 빅 데이터 분석, 모바일과 게임 어플리케이션, 콘텐츠 배포
	- infrequent Access
		- 자주 액세스하지 않지만 필요 시 빠르게 액세스해야하는 데이터
		- standard보다 비용은 적으나 검색 비용이 발생
		- standard-IA는 가용성이 99.9%로 standard보다 떨어짐
		- 재해 복구와 백업에 사용
	- one zone-infrequent access
		- one zone-IA
		- 단일 AZ에서 높은 내구성을 갖지만 AZ 장애 시 데이터를 잃음
		- 온프레미스 데이터를 2차 백업하거나 재생성 가능한 데이터를 저장하는데 사용
	- glacier instant retrieval
		- Glacier은 콜드 스토리지이고 비용은 스토리지 비용과 검색 비용이 발생
		- 최소 보관 기간이 90일이지만 밀리초 이내에 액세스 해야 하는 경우
		- 데이터 검색이 `즉각적`
	- glacier Flexible retrieval
		- glacier instant retrieval와 비슷하지만 데이터 검색 시 최대 12시간까지 걸림
	- glacier Deep Archive
		- 데이터 장기 보관을 위한 스토리지 - 데이터를 아카이브화
	- intelligent Tiering
		- 검색 비용이 없음
		- 알아서 객체를 이동시켜줌
- 내구성(durability) 보장: 객체 저장 시 천만개를 저장할 때 1개 손실이 나는 수준
	- 모든 스토리지 클래스의 내구성은 동일
- 가용성(availiability): 얼마나 용이하게 제공되는가, standard는 1년에 53분 정도 에러가 발생함
	- AZ가 적어질 수록 낮아짐
 - General Purpose
	 - 일반적
- Infrequent Acess
	-  비용이 적게 들고, 자주 접근하지 않을 경우
	- 재해 복구 등에 사용
- Glacier Storage
	- 저비용 아카이브 및 백업용
