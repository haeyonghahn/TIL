# IAM
해당 문서는 [AWS(Amazon Web Service) 입문자를 위한 강의](https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8/dashboard)를 토대로 작성되었습니다.

## 목차
  * **[IAM(Identity and Access Management)이란?](#IAM(Identity-and-Access-Management)이란?)**
  * **[IAM 정책 시뮬레이터](#IAM-정책-시뮬레이터)**
  * **[실습](#실습)**

## IAM(Identity and Access Management)이란?
유저를 관리하고 접근 레벨 및 권한에 대한 관리
- 접근키(Access Key), 비밀키(Secret Access Key)
- 매우 세밀한 접근 권한 부여 기닝(Granular Permission)
- 비밀번호를 수시로 변경 가능하게 해준다.
- Multi-Factor Authentication(다중 인증) 기능 제공

- 그룹(Group)
- 유저(User)
- 역할(Role)
- 정책(Policy)

- 정책은 그룹, 역할에 추가시킬 수 있다.
- 하나의 그룹 안에 다수의 유저가 존재 가능하다.
- IAM은 지역 설정이 필요 없다.

## IAM 정책 시뮬레이터
1. 개발환경(Staging or Develop)에서 실제환경(Production)으로 빌드하기전 IAM 정책이 잘 작동되는지 테스트하기 위함이다.
2. IAM과 관련된 문제들을 디버깅하기에 최적화된 툴이다.(이미 실제로 유저에 부여된 다양한 정책들도 테스트 가능하다.)

![tempsnip](https://user-images.githubusercontent.com/31242766/214997346-b4f0266d-4909-4b7b-9023-544def058193.png)
![image](https://user-images.githubusercontent.com/31242766/214997419-11f0e404-1600-4a8a-91f8-8272c79519dd.png)

## 실습
### 사용자 추가
- IAM -> 사용자 -> 사용자 생성   
![image](https://user-images.githubusercontent.com/31242766/215008132-9a6b9ece-245c-411d-8a54-756bbd3f5084.png)
![image](https://user-images.githubusercontent.com/31242766/215008193-25c5b49d-f781-45dd-9871-6328a6348b33.png)

### 액세스 키 생성
- IAM -> 사용자 -> aws_learner
![image](https://user-images.githubusercontent.com/31242766/215009203-95bd8cd8-5f56-4e44-9f6b-ed21f2c54ca2.png)

### 사용자 그룹 생성
- IAM -> 사용자 그룹 -> 사용자 그룹 생성   
사용자 그룹에 추가시킬 사용자 및 권한 정책을 선택한다.   
![image](https://user-images.githubusercontent.com/31242766/215009501-0de06ad2-4ae2-4ae8-9306-d4484f5d4f79.png)

### 사용자 그룹 추가
- IAM -> 사용자 -> aws_learner -> 그룹에 사용자 추가
![image](https://user-images.githubusercontent.com/31242766/215009878-6a793d38-ba73-4a48-8f85-0ea03ff93a32.png)

### 역할 생성
- IAM -> 역할 -> 역할 생성
![image](https://user-images.githubusercontent.com/31242766/215010127-b1cb9bdf-7522-4784-8ff7-39833e94cbe1.png)
![image](https://user-images.githubusercontent.com/31242766/215011451-33a5eeb5-b066-44a8-9f9f-e4c3f8b7400c.png)

### 정책 생성
IAM -> 정책
![image](https://user-images.githubusercontent.com/31242766/215011920-73ff58ec-3b9e-40e9-b320-3e38a59466c0.png)
![image](https://user-images.githubusercontent.com/31242766/215013079-660a0e98-c09b-45be-9574-a9603b333270.png)
![image](https://user-images.githubusercontent.com/31242766/215013604-55ef4565-ef17-41f8-b37d-68e2a0bf496a.png)
![image](https://user-images.githubusercontent.com/31242766/215013762-c73c1f82-22ef-452c-81a0-dd96c5b3ac71.png)

### IAM 정책 시뮬레이터
![image](https://user-images.githubusercontent.com/31242766/215013875-7551085d-9b2a-4464-ad3d-effb16bdb711.png)   
- aws_learner 사용자에게 dynamoDB 정책이 부여되지 않았을 때   
![image](https://user-images.githubusercontent.com/31242766/215013990-6e203c6a-35cc-4bab-8afb-473a0bf7fb2d.png)    
- aws_learner 사용자에게 dynamoDBReadOnlyAccess 정책을 부여하여 확인해보았을 때      
![image](https://user-images.githubusercontent.com/31242766/215014193-eebb4b1f-a8c3-4c75-bfb5-6750f706feda.png)
![image](https://user-images.githubusercontent.com/31242766/215014331-c4b6b8ba-631e-409f-9bb4-4579cd55ce62.png)
![image](https://user-images.githubusercontent.com/31242766/215014363-ca96722a-d5af-4f0f-a1d0-dacbc6123ee3.png)
![image](https://user-images.githubusercontent.com/31242766/215014476-7dbdc13b-e737-4507-82b8-b4b0f5432294.png)
![image](https://user-images.githubusercontent.com/31242766/215014543-494c9cce-6e04-475e-a128-92bfd714ec9e.png)
