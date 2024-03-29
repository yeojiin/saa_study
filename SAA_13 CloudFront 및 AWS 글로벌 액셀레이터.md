### CloudFront 개요
- CDN, 즉 컨텐츠 전송 네트워크
-  **웹사이트의 컨텐츠를 서로 다른 엣지 로케이션에 미리 캐싱하여 읽기 성능을 높이는 것**
	- 컨텐츠가 **네트워크 전체에 캐싱** 되기 때문에 전세계 사용자들이 **낮은 레이턴시로** 접근가능
	- 호주 S3 버킷에 웹사이트를 구축해도 미국 사용자가 사용할 경우 cloudFront를 통해 미국에 있는 엣지에 컨텐츠를 요청함 -> cloudfront가 호주에서 해당 컨텐츠를 패치해서 가져옴 -> 이후 미국 사용자가 같은 컨텐츠를 요구하면 호주에서 가져오지 않고 엣지에서 직접 캐싱된 컨텐츠를 제공받음
- 컨텐츠가 전체적으로 분산되어 DDoS 공격에서 보호받을 수 있음
	- Shield와 어플리케이션 방화벽
- 작동방식
	-  S3 버킷으로, CloudFront를 통해 파일을 분산하고 캐싱
	-  기존의 OAI를 대체하는 OAC(Origin Access Control)을 이용해 **버킷에는 CloudFront만 접근 가능**
	- CloudFront를 통해 버킷에 데이터를 보내는 방법도 가능 = Ingress
	- HTTP 백엔드와 같은 사용자 정의 원본을 쓸 수도 있음
		- ex) EC2인스턴스, 로드밸런스, S3웹사이트
		- 단, S3 웹사이트의 경우 버킷을 활성화 해 정적 웹사이트로 설정해야 함
- 작업 방식
	- 전세계 CloudFront의 **엣지가** 있고 여기에 연결할 원본이 있음 -> S3 버킷이나 HTTP 서버
	-  클라이언트가 연결해서 엣지 로케이션에 HTTP 요청을 보내면, 엣지는 이것이 캐싱되어 있는지 확인
	-  캐싱되어 있지 않을 경우에는, 원본으로 가서 요청 결과를 가져오면서 이를 로컬 캐시에 저장해 다른 클라이언트가 같은 컨텐츠를 같은 엣지에 요청할 경우 재사용
	- S3를 원본으로 사용할 경우 AWS 클라우드의 어떤 리전에 있는 S3 버킷이 원본임
	- 사용자는 엣지가 직접 컨텐츠를 제공
		- 내부 AWS망을 통해 S3 버킷 원본을 받아옴
		- 이 버킷은 OAS로 보호받으며, S3 버킷 정책에 의해서만 수정 가능
-  CloudFront와 CRR(교차 리전 복제)간의 차이점
	- CloudFront는 전세계의 엣지 네트워크를 이용
		- 전세계를 대상으로 한 정적 컨텐츠를 사용하고자 할 때 용이
	- 교차 리전은 복제를 원하는 각 리전에 이 설정이 필요 -> 전세계 대상 x, 파일은 거의 실시간으로 갱신 -> 캐싱되지 않고 **읽기 전용으로만 설정 가능**
		- 일부 리전을 대상으로 동적 컨텐츠를 낮은 지연 시간으로 제공 할때
### ALB or EC2 as on Origin
- CloudFront는 사용자 지정 HTTP 백엔드에도 접근 가능
	- EC2 인스턴스나 어플리케이션 로드 밸런서도 포함
- EC2 인스턴스 통해 HTTP 백엔드를 개발 가정
	- 사용자들이 CloudFront의 엣지 로켓이션을 통해 접근
	-  어플리케이션이 EC2 인스턴스에 요청을 보냄
	- 이 둘은 모두 **퍼블릭으로** 설정 되어야 함 -> 아니면 CloudFront에는 가상 프라이빗 클라우드가 없기 때문에 EC2 인스턴스에 접근할 수 없음
	- 엣지 로케이션의 모든 공용 IP가 EC2에 접근할 수 있도록 하는 **보안 그룹 필요**
- 어플리케이션 로드 밸런서 이용
	-  Public 설정 필요
	- 백엔드의 EC2 인스턴스는 프라이빗 설정 가능 ->  로드 밸런서와 인스턴스 간에 가상 프라이빗 네트워크가 설정되어 있어서
	- EC2 인스턴스의 보안 그룹이 로드밸런서를 허용하면 됨
	- 어플리케이션의 공용 IP가 로드 밸런서의 보안 그룹 정책에 허용 필요
### CloudFront 지리적 제한
- 이 기능을 이용하면 사용자들의 지역에 따라 여러분들의 배포 객체 접근을 제한 가능
- 접근이 가능한 국가 목록을 만들거나, 반대로 접근이 불가능한 국가 목록을 설정
- 이 국가는 서드 파티 지역 DB에서 설정한 것으로, 사용자의 IP가 어떤 국가에 해당하는지 확인 가능
### ### CloudFront Price Classes
- 요금(Pricing)과 가격 등급(Price Class)
- 엣지 로케이션마다 데이터 전송 비용이 다름
- CloudFront에서 더 많은 데이터가 전송될수록 비용은 낮아짐
- 비용 절감을 위해 여러분의 CloudFront를 분산할 전 세계 엣지 로케이션 수를 줄이는 방법
- Price Class All
	- 모든 리전 사용
	- 최고 성능 제공
	- 가장 비싼 리전은 제외
- Price Class 100
	- 북미와 유럽 엣지 로케이션 제공
	- 가장 저렴한 리전만 사용
- Price Class 200
	-  대부분의 리전을 사용하지만 가장 비싼 리전은 제외됨
### Cache Invalidation(캐시 무효화)
-  CloudFront에는 항상 백엔드 오리진이 있음 -> CloudFront 엣지 로케이션은 우리가 백엔드 오리진을 업데이트할 때 업데이트 사항을 알 수 없음
-  캐시의 타임 투 리브(TTL)가 만료되면 백엔드 오리진으로부터 업데이트된 콘텐츠를 받음 -> 새로운 컨텐츠를 받지 못할 수 있음
-  캐시를 강제로 새로고침해서 캐시에 있는 타임 투 리브(TTL)를 모두 제거할 수 있음
- `CloudFront 무효화`를 실행 필요
	- 특정 파일 경로를 전달 필요
	- 별표(\*\)로 시작하는 파일이나 /images/\*\와 같은 특정 경로를 무효화할 수 있음
- 작동원리
	-  여러 엣지 로케이션 CloudFront를 분산
	-  각 엣지 로케이션이 가진 고유의 캐시에는 index.html 파일과 오리진인 S3 버킷에서 가져온 이미지가 있다고 가정
	-  이 파일들의 타임 투 리브(TTL)를 하루로 설정 가능
	- 1.  특정 파일을 무효화하는 /index.html
	- 2. 엣지 로케이션의 캐시에 있는 모든 이미지를 지우는 /images/
	- CloudFront는 캐시에서 이 두 파일을 무효화하도록 엣지 로케이션에 플래그
	- 엣지 로케이션에서 CloudFront로 다시 조회 요청을 보내면 캐시가 없어서 새로 업데이트된 파일을 받음
### Gloabl Accelerator
- 애플리케이션을 배치했다고 가정
	- 글로벌 애플리케이션이고 글로벌 사용자들이 직접 접근
	-  애플리케이션은 오직 한 리전에 배치됨
	- 사용자들이 어플리케이션에 접근할 때 공용 인터넷을 통하는데 라우터를 거치는 동안의 수 많은 홉으로 인해 상당한 지연이 발생
	- 지연 시간을 최소화하기 위해 최대한 빨리 AWS 네트워크를 통하는 것이 유리
	- Global Accelerator를 사용
-  유니캐스트 IP(Unicast IP)
	- 하나의 서버가 하나의 IP 주소를 가짐
	- 클라이언트가 두 개의 서버와 통신할 때 왼편 서버의 IP 주소는 숫자 12로 시작하고, 반대편은 98로 시작 -> 각각 이중화된 서버 접근 가능
- 애니캐스트 IP(Anycast IP)
	-  모든 서버가 동일한 IP 주소를 가짐
	-  클라이언트는 가장 가까운 서버로 라우팅
- Global Accelerator는 이 `애니캐스트 IP` 개념을 사용
	- 애플리케이션을 라우팅하기 위해 AWS 내부 글로벌 네트워크를 활용
	-  가장 가까운 엣지 로케이션과 통신
	- 엣지 로케이션으로부터 내부 AWS 네트워크를 거쳐 ALB로 곧장 연결
	- 탄력적 IP, EC2 인스턴스 애플리케이션 로드 밸런서 네트워크 로드 밸런서와 함께 작동하며 공용일 수도 있고 사설일 수도 있음
	- 네트워크를 거치기 때문에 안정적인 성능
	- intelligent routing 이기 때문에 지연 시간이 가장 짧은 엣지 로케이션에 연결되며 리전 장애 조치도 신속히 이루어짐
- 상태 확인
	- 클라이언트가 화이트리스트 해야 하는 단 두개의 외부 IP만 존재하기 때문에 보안에서 안전
	- Global Accelerator가 애플리케이션에 대해 상태 확인을 실행
	- 애플리케이션이 글로벌한지 확인
	- 한 리전에 있는 한 ALB에 대해 상태 확인을 실패하면 자동화된 장애 조치가 1분 안에 정상 엔드 포인트로 실행
	- 복구에 특히 뛰어남
- Global Accelerator와 CloudFront 사이의 차이점
	- 공통점
		- Global Accelerator와 CloudFront는 둘 다 `동일한 글로벌 네트워크를 사용`하고 둘 다 AWS가 생성한 `전 세계의 엣지 로케이션을 사용`
		- DDoS 보호를 위해 둘 다 `AWS Shield와 통합`
	- CloudFront는 이미지나 비디오처럼 캐시 가능한 내용과 API 가속 및 동적 사이트 전달 같은 **동적 내용 모두에** 대해 성능을 향상 -> `엣지 로케이션`으로부터 제공
	- Global Accelerator는 TCP나 UDP상의 다양한 애플리케이션 성능을 향상
	- 패킷은 엣지 로케이션으로부터 하나 이상의 AWS 리전에서 실행되는 애플리케이션으로 프록시 됨
	- 이 경우에는 모든 요청이 애플리케이션 쪽으로 전달 -> 캐싱은 불가능
	- 게임이나 IoT 또는 Voice over IP 같은 비 HTTP를 사용할 경우에 적합
	- 글로벌하게 고정 IP를 요구하는 HTTP를 사용할 때도 매우 유용
	- 결정적이고 신속한 리전 장애 조치가 필요할 때
