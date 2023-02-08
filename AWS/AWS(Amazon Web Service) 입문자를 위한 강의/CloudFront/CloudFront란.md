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

## 실습
### S3 버킷 생성
- Amazon S3 -> 버킷 -> 버킷 만들기    
![image](https://user-images.githubusercontent.com/31242766/217435645-899acdc2-702f-4de4-88e8-da1c24bb9284.png)
![image](https://user-images.githubusercontent.com/31242766/217435847-ef85c04a-189f-4324-9c95-e77652508b6f.png)
![image](https://user-images.githubusercontent.com/31242766/217435909-d2e32c83-092d-4c18-9d2a-c7bfdb248e7f.png)

- Amazon S3 -> 버킷 -> aws-learner-cloudfront   
![image](https://user-images.githubusercontent.com/31242766/217437180-d0665595-5d3d-4f95-89fe-599f1e6a5523.png)

- Amazon S3 -> 버킷 -> aws-learner-cloudfront -> 권한    
S3 객체 URL 접근을 위한 정책을 설정하자.      
![image](https://user-images.githubusercontent.com/31242766/217445408-5ee62aad-ad2d-49ff-a10b-2aa9a982f510.png)

### cloudFront 생성
- CloudFront -> 배포 -> create
![image](https://user-images.githubusercontent.com/31242766/217447583-00c26d15-5056-400c-a047-d043d5a54253.png)
![image](https://user-images.githubusercontent.com/31242766/217447694-d65e49be-bd45-4985-adb8-6cb037575c03.png)
![image](https://user-images.githubusercontent.com/31242766/217447738-4e40a44d-a88f-4c92-aec9-49450299e852.png)
![image](https://user-images.githubusercontent.com/31242766/217447779-9445680d-6403-45ec-aeab-7d21a99fb838.png)
![image](https://user-images.githubusercontent.com/31242766/217447850-d6673e8e-eca2-4731-856a-702c38686ab2.png)

- https://d1mquxwev70in2.cloudfront.net/pngwing.com.png 접속   
![tempsnip](https://user-images.githubusercontent.com/31242766/217448252-6c038e40-f73a-4579-aeca-50afd83841a4.png)


