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

## 사용 사례 - 2
