# DynamoDB
해당 문서는 [AWS(Amazon Web Service) 입문자를 위한 강의](https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8/dashboard)를 토대로 작성되었습니다.

- NoSQL(Not Only SQL) 데이터베이스
- 매우 빠른 쿼리 속도
- Auto-Scaling 기능 탑재
- Key-Value 데이터 모델 지원
- 테이블 생성시 스키마 생성 필요 없음
- 모바일, 웹, IoT데이터 사용시 추천됨
- SSD 스토리지 사용

## DynamoDB 구성
![image](https://user-images.githubusercontent.com/31242766/217526834-ac9ed2d8-63e4-494d-8ce5-a89c8dd2b77c.png)   
- 테이블 (Table)
- 아이템 (Items) - 행(row)과 개념이 비슷함
- 특징 (Attributes) - 열(column)과 개념이 비슷함
- Key-Value (Key : 데이터의 이름, Value : 데이터 자신)
- 예시) JSON, XML

## DynamoDB - Primary Keys (PK)
- PK를 사용하여 데이터 쿼리
- DynamoDB에는 두가지의 PK 유형이 있음
  - 파티션키 (Partition Key)   
    ![image](https://user-images.githubusercontent.com/31242766/217528033-e5b9242d-3765-47d5-8a5b-0d56d3fec43e.png)   
    - 고유 특징 (Unique Attribute)
    - 실제 데이터가 들어가는 위치를 결정해줌
    - 파티션키 사용시 동일한 두개의 데이터가 같은 위치에 저장될 수 없음
  - 복합키 (Composite Key)   
    ![image](https://user-images.githubusercontent.com/31242766/217528631-1ed6c520-8d79-4fe3-a635-cbff8f65a12e.png)
    - 파티션키(Partition Key) + 정렬키(Sort Key)
    - 예시 : 똑같은 고객이 다른 날짜에 다른 물건을 구매
    - 파티션키 : 고객아이디, 정렬키 : 날짜(Timestamp)
    - 같은 파티션키의 데이터들은 같은 장소에 보관, 그다음 정렬키에 의해 데이터가 정렬됨
- 한 고객이 다른 날짜에 다른 아이템을 구매한다고 가정해보자. 이러한 경우에 `Customer_id` 로만 키를 잡을 수 없으므로 `Transaction_date` 복합키를 사용한다.
```json
{
  "Customer_id" : "28942",
  "Transaction_id" : "g9s4dd2",
  "Item_purchased" : "sofa",
  "Store_location" : "seoul",
  "Transaction_date" : "2020-10-16 14:20:00"
}
```

## DynamoDB 데이터 접근 관리
- AWS IAM으로 관리할 수 있음
- 테이블 생성과 접근 권한을 부여할 수 있음
- 특정 테이블만, 특정 데이터만 접근 가능케 해주는 특별한 IAM 역할 존재
- 예를 들어, 유저 아이디와 등수가 나올 때 다른 사람들의 데이터는 보이지 않게끔 가려야하는 경우가 있다. 이러한 경우 simonKim인 가지고 있는 유저에게 하나의 데이터만 보여주는 
  특수 IAM 역할을 부여함으로써 가능하게 할 수 있다. 여기서는 파티션키는 `유저 아이디`가 된다.
![image](https://user-images.githubusercontent.com/31242766/217530532-5f1614bd-2e25-4489-813b-52bda651797a.png)
