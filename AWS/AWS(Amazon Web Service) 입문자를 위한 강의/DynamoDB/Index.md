# Index
해당 문서는 [AWS(Amazon Web Service) 입문자를 위한 강의](https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8/dashboard)를 토대로 작성되었습니다.

- 특정 컬럼만을 사용하여 쿼리
- 테이블 전체가 아닌 기준점(pivot)을 사용해 쿼리가 이루어짐
- 매우 큰 쿼리 성능 효과
- 두가지의 Index 유형 존재
  - Local Secondary Index
  - Global Secondary Index 

## Local Secondary Index (LSI)
- 테이블 생성시에만 정의해줄 수 있음
- 따라서 테이블 생성 후 변경, 삭제가 불가능
- 똑같은 파티션키 사용, 그러나 다른 정렬키 사용
- 예를 들어, 메인 테이블을 가지고 두 가지 뷰를 생성했다고 하자. X 라는 뷰는 2019년도 데이터, Y 라는 뷰는 2020년도 데이터이다. 파티션키는 X 와 Y는 동일하다. 
  LSI는 파티션키와 정렬키를 사용하기 때문에 정렬키를 기반으로 쿼리 수행 시 빠르게 수행되어진다. 만약 정렬키가 없고 2020년도 데이터만 가지고오고 싶다면 테이블 전체를 
  찾아야 할 것이다. 하지만 `년도`로 정렬키가 정의되어 있다면 쿼리 속도가 빨라진다. 이것이 `Local Secondary Index` 이다.   
  ![image](https://user-images.githubusercontent.com/31242766/219361199-4a15a141-e450-4148-b8e2-6b8e02513525.png)

## Global Secondary Index (GSI)
- 테이블 생성후에도 추가, 변경, 삭제 가능
- 다른 파티션키, 정렬키 사용
  ![image](https://user-images.githubusercontent.com/31242766/219361495-90aef842-911e-40a5-a09b-88f6470598b4.png)
