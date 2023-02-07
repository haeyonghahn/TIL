# Lambda
## AWS Lambda (기본)
- Serverless의 주축을 담당
- Events를 통하여 Lambda를 실행시킴
- NodeJS, Python, Java, GO 등 다양한 언어 지원
- Lambda Function

## AWS Lambda (비용)
- Lambda Function이 실행될 때만 돈 지불
- 매달 1,000,000 함수 호출 시 무료 (그 이후로는 유료)

## AWS Lambda (기타)
- 최대 300초(5분) 런타임 시간 허용
- 512MB의 일시적인 디스크 공간 제공(/tmp)
    - Lambda 함수로 돌아오는 Input을 일시적인 공간에 저장할 때 코딩을 통하여 임시 저장소에 넣어두고 꺼내어 사용할 수 있다.
      주로 `data preprocessing`시 사용되어진다.
- 최대 50MB Deployment Package 허용
    - AWS콘솔에서 직접 코딩하여 짤 수 있지만 로컬에서 다수의 파일을 하나의 압축 파일로 저장한 다음
      AWS에 업로드하여 Deployment를 통하여 사용할 수 있다. 50MB를 넘기면 업로드되지 않지만 50MB가 초과될 시에 
      S3 버켓에 업로드한 후 Lambda에서 파일에 대한 경로를 지정하여 사용할 수 있다.

## 사용 사례 - 1
![image](https://user-images.githubusercontent.com/31242766/217238985-a6e5ab37-696e-492a-b3ff-94d2c0fb7a1c.png)

## 사용 사례 - 2
![image](https://user-images.githubusercontent.com/31242766/217239023-09d1cb97-aba2-4ba4-a074-ade80c6520a4.png)

## 실습 1
![image](https://user-images.githubusercontent.com/31242766/217239672-36925a32-cc8a-4ec1-a3bc-b0a165a9bcca.png)
- 동시성 : 얼마나 많은 함수를 같은 요청이 들어왔을 때 돌릴 수 있을지에 대한 것이다. 설정에 놓은 동시성 갯수보다 더 많은 함수가 발생된다면 함수는 정상적으로 동작되지 않을 것이다.

- Lambda -> 함수 -> 함수 생성
![image](https://user-images.githubusercontent.com/31242766/217240798-cbfa3573-c0f7-410b-9d15-bb48ac8ccbc4.png)

- 테스트 이벤트 구성   
Lambda함수에 보내고 싶은 값을 전달할 수 있다.   
![tempsnip](https://user-images.githubusercontent.com/31242766/217241447-bac9abc9-de21-4fac-b866-1414db6764ab.png)
![image](https://user-images.githubusercontent.com/31242766/217241769-c89d46f1-0477-4d2a-89c0-679c77d5186b.png)
![image](https://user-images.githubusercontent.com/31242766/217242060-7c02f3cd-8283-488e-a099-7c78ac3c9280.png)   
- `Request ID` : UUID 포맷이며 매번 Lambda함수가 실행될 때 생성되는 고유 ID이다. 해당 ID를 통해 CloudWatch에서 검색해서 로그를 찾을 수 있다.
- `Deploy` 버튼은 코드를 수정하고나서 버튼을 클릭해야 수정된 코드가 반영이 된다.

## 실습 2
