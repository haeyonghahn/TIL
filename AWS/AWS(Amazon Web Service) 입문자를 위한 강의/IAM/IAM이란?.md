# IAM
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
