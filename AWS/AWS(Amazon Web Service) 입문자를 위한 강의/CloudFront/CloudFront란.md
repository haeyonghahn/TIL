# CloudFront
- 정적, 동적, 실시간 웹사이트 컨텐츠를 유저들에게 전달
- Edge Location을 사용
- 컨텐트 딜리버리 네트워크 Content Delivery Network(CDN)
- 분산 네트워크 (Distributed Network)

![image](https://user-images.githubusercontent.com/31242766/217267192-4c07d0c5-f81f-4ac4-a6da-009147133ecd.png)
![image](https://user-images.githubusercontent.com/31242766/217267243-533f27b5-e021-4400-b58c-5bed98145386.png)

## CloudFront 용어 정리
- Edge Location (엣지 지역) : 컨텐츠들이 캐시(Cache)에 보관되어지는 장소
- Origin (오리진) : 원래 컨텐츠들이 들어있는 곳, 웹서버 호스팅이 되어지는 곳. S3, EC2인스턴스가 오리진이 될 수 있음
- Distribution(분산) : CDN에서 사용되어지며 Edge Location들을 묶고 있다는 개념
