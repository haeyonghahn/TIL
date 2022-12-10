# MySQL 성능 최적화
## 목차
* **[인덱스를 왜 쓸까?](#인덱스를-왜-쓸까?)**
  * **[조회에 이득을 얻고 수정/삭제에서 손해를 보는데 괜찮나요?](#조회에-이득을-얻고-수정/삭제에서-손해를-보는데-괜찮나요?)**
  * **[ODER BY, GROUP BY에서 이점](#ODER-BY,-GROUP-BY에서-이점)**
* **[인덱스 실행 계획](#인덱스-실행-계획)**
  * **[all](#all)**
  * **[range](#range)**
  * **[index](#index)**
* **[인덱스 적용 사례1](#인덱스-적용-사례1)**
  * **[대체 어느 컬럼에 인덱스를 걸어야 할까?](#대체-어느-컬럼에-인덱스를-걸어야-할까?)**
* **[인덱스 적용 사례2](#인덱스-적용-사례2)**
  * **[복합 인덱스](#복합-인덱스)**
* **[인덱스 적용 사례3](#인덱스-적용-사례3)**
  * **[커버링 인덱스](#커버링-인덱스)**
* **[인덱스 적용 사례4](#인덱스-적용-사례4)**
  * **[인덱스 컨디션 푸시다운](#인덱스-컨디션-푸시다운)**

## 인덱스를 왜 쓸까?
DB에서 성능 최적화는 디스크 I/O와 관련이 많다. 만약 조회 성능 개선을 한다면 디스크 I/O를 줄이는 것이 핵심이 될 것이다. 원하는 곳에 데이터를 읽기 위해서 실제로 물리적인 디스크가 돌아야 하고 디스크 헤더가 움직여야 한다. 이 과정에서 물리적인 움직임이 있기 때문에 데이터의 입출력은 느리다. 실제로 하드 디스크 I/O와 메모리 I/O의 속도 차이는 10만~15만 배 정도이고 이 차이는 달팽이와 전투기의 속도 차이다. 요즘 SSD가 많이 보편화되었지만 그래도 여전히 메모리 I/O에 비해서는 많이 느리다. 즉, 성능 개선을 한다는 것은 디스크 I/O를 줄이는 것이 핵심이다.
### 조회에 이득을 얻고 수정/삭제에서 손해를 보는데 괜찮나요?
인덱스를 쓰면 조회는 빨라지지만 데이터 수정, 삭제, 생성은 느려진다. 그럼에도 불구하고 인덱스를 왜 생성해서 써야 하는가? 일반적으로 웹 서비스의 경우 CRUD에서 R(Read)과 CUD(Create, Update, Delete)의 비율이 8:2 에서 9:1 정도이다. 그래서 조회에서 성능 최적화를 하고 데이터 수정, 삭제는 조금 손해를 보더라도 전체적으로 이득을 보자는 취지이다.
### ODER BY, GROUP BY에서 이점
#### ORDER BY : 인덱스를 이용해 정렬이 처리되는 경우
nickname에 의해서 ORDER BY, 즉 정렬을 하고 있다. 만약에 인덱스가 없었다면 데이터를 다 읽어 와서 DB에서 직접 정렬했어야 할 것이다. 하지만 인덱스는 이미 정렬되어 있기 때문에 인덱스 순서대로 파일을 읽기만 하면 된다.
```sql
SELECT *
FROM crew
WHERE nickname >= '매트' AND nickname <= '토르'
ORDER BY nickname;
```
#### GROUP BY : 인덱스를 이용해 GROUP BY를 하는 경우 
GROUP BY도 비슷하다. 각 track에서 nickname이 가장 빠른 사람들을 가져오는 쿼리를 날린다고 하자. 인덱스가 걸려 있는 경우에는 프론트에서 nickname이 가장 빠른 '꼬재'를 읽고 나머지 데이터는 읽지 않고 바로 백엔드 track로 넘어가서 '매트'만 읽으면 된다. 여기서 중간 부분을 읽지 않았기 때문에 디스크 I/O를 많이 줄일 수 있다.
```sql
CREATE INDEX idx_crew_track_nickname ON crew (track, nickname);
SELECT track, MIN(nickname) FROM crew GROUP BY track;
```
![image](https://user-images.githubusercontent.com/31242766/202222693-7812ad29-d008-48ee-9d4d-9436f4f10fe3.png)

![image](https://user-images.githubusercontent.com/31242766/202222730-41d3269c-8dd2-4656-95e3-713df6b4523f.png)

## 인덱스 실행 계획
실행 계획은 여러 가지가 있지만 3가지 정도를 알아보자. 
- `all` : 테이블 전체를 스캔할 때
- `range` : 인덱스를 이용하여 범위 검색을 할 때
- `index` : 인덱스 전체를 스캔할 때

### all
`Full Table Scan`을 의미한다. 아래 보이는 것처럼 데이터를 하나하나 다 읽는 것을 말한다. 시간이 많이 걸리는 작업이기 때문에 `Full Table Scan`을 하면 성능이 좋지 않다. `Full Table Scan`이 일어나는 경우는 두 가지가 있다.
- 인덱스가 없어서 Full Table Scan을 하는 경우
- 인덱스가 있는데도 Full Table Scan을 하는 경우
  - 해당 경우는 데이터 전체의 개수가 그렇게 많지 않거나 읽고자 하는 데이터가 전체 데이터의 25%를 넘어가면 인덱스가 있다 하더라도 Full Table Scan이 일어난다.

![image](https://user-images.githubusercontent.com/31242766/202223413-af8dc900-5e00-473b-9d12-16b03375307e.png)

### range
이상적으로 인덱스를 잘 걸었을 때 발생하는 실행 계획이다. 이전에 데이터 전체를 다 읽었던 것과는 달리 필요한 부분만 데이터를 읽게 된다. 즉, 탐색할 데이터를 줄이기 때문에 디스크 I/O를 줄일 수 있다. 

![image](https://user-images.githubusercontent.com/31242766/202224420-f7c8b039-9cb2-4f84-8fa8-1b0c5a12ffba.png)

### index
Index Full Scan이기 때문에 전체 인덱스를 다 읽게 된다. 이전에 봤던 Full Table Scan보다 성능이 좋긴하다. 왜냐하면 인덱스는 데이터 파일보다는 크기가 작기 때문이다. 그럼에도 불구하고 Index Range Scan보다는 성능이 좋지 않다.

![image](https://user-images.githubusercontent.com/31242766/202225112-3c0b5893-e37f-4069-8ca3-695bb3562f29.png)

## 인덱스 적용 사례1
### 대체 어느 컬럼에 인덱스를 걸어야 할까?
1. 서비스의 특성상 무엇에 대한 `조회`가 많이 일어나는지 파악
2. `카디널리티`가 높은 `컬럼`에 대해 인덱스를 생성

id의 경우 InnoDB 이므로 클러스터 인덱스(실제 데이터가 인덱스 정렬 순서로 정렬)가 미리 걸려있을 것이다. nickname, track, age가 있는데, 인덱스를 걸기 위해서는 서비스의 특성상 
무엇에 대한 조회가 많이 일어나는지를 우선 파악해야 한다. 만약 서비스에서 nickname에 대한 조회가 많이 일어난다고 가정해보자. 그 다음에는 `Cardinality(특정 집합의 유일한 값의 개수)`가 높은 컬럼에 대해서 인덱스를 생성해야 한다. 예를 들어, nickname에 값이 중복이 일어나지 않고 있고 track에 값이 중복이 많이 일어난다고 가정한다면 nickname에 인덱스를 거는 것이 좋다.

![image](https://user-images.githubusercontent.com/31242766/202225571-ce894dd5-b0a9-41c2-9b49-052e9b0d3534.png)

```sql
CREATE INDEX idx_crew_nickname ON crew (nickname);
```
처음에 실행 계획이 ALL, 즉 Full Table Scan이었다가 인덱스를 걸고 나면 Index Range Scan으로 바뀐 것을 알 수 있다. 실제 성능도 줄어든다.

![image](https://user-images.githubusercontent.com/31242766/202226832-9044fe55-c160-47b6-a426-1bcbc91290f8.png)

![image](https://user-images.githubusercontent.com/31242766/202227410-19acf4eb-9997-4641-9d3d-6dc0a48cf1bf.png)

## 인덱스 적용 사례2
### 복합 인덱스
`복합 인덱스`란, 두 개 이상의 컬럼을 합쳐서 인덱스를 만드는 것이다. 하나의 컬럼으로 인덱스를 만들었을 때보다 더 적은 데이터 분포를 보여 탐색할 데이터 수가 줄어든다. 결합 인덱스, 다중 컬럼 인덱스, Composite Index라고도 불린다. 

해당 데이터가 나이(age)순, 다음에는 nickname 순으로 정렬되어 있다고 하자. 

![image](https://user-images.githubusercontent.com/31242766/202228015-9e577086-8c94-4b25-9a17-f352a2c4eff8.png)

![image](https://user-images.githubusercontent.com/31242766/202228046-20c0ed28-5e5d-4d4c-836b-892086212717.png)

이렇게 정렬된 기준으로 아래의 쿼리를 날렸을 때, 탐색 범위를 줄일 수 있을지 생각한다면 복합 인덱스를 쓸 수 있을지 없을지 알 수 있다. 현재 나이 순으로 정렬되어 있기 때문에 26살부터 데이터를 탐색하면 된다. 
```sql
SELECT * FROM crew WHERE age >= 26;
```
![image](https://user-images.githubusercontent.com/31242766/202228604-c07cf9da-f641-48dc-ba22-22d4c5e9d607.png)

그리고 nickname이 '토르'보다 뒤에 나오는 사람들을 가져 오려고 한다. 마찬가지로 나이(age)순으로 정렬되어 있고 nickname 순으로 정렬되어 있어서 '토르'이후인 사람들을 가져오면 되기 때문에 이번에도 탐색 범위가 줄었다. 
```sql
SELECT * FROM crew WHERE age >= 26 AND nickname >= '토르';
```
![image](https://user-images.githubusercontent.com/31242766/202229368-9adf8d73-2a79-4b54-9386-9b971f4ebd1e.png)

만약 nickname을 기준으로 탐색하고자 하면 어떻게 될까? 여기서 보면 nickname이 '동키콩'이후인 사람들 '티커', '파랑' 등 등은 해당 기준으로 원하는 만큼 데이터 탐색 범위를 줄일 수 없다. 따라서 Full Table Scan을 하게 된다. 즉, 디스크 I/O를 줄일 수 없게 된다. 따라서 이러한 것들을 잘 고려해서 복합 인덱스를 생성해야 한다.
```sql
SELECT * FROM crew WHERE nickname >= '동키콩';
```

## 인덱스 적용 사례3
### 커버링 인덱스
인덱스를 사용하여 처리하는 쿼리 중 가장 큰 부하를 차지하는 부분은 어디일까. 인덱스 검색에서 일치하는 키 값의 `레코드를 읽는 것이다.`

아래의 그림을 살펴보면 인덱스 검색에서 일치하는 키 값의 데이터를 읽기 위해서 추가적인 디스크 I/O가 발생하게 된다. N개의 인덱스를 검색할 때 최악의 경우 N번의 디스크 I/O가 발생할 수 있다. 쿼리 최적화의 가장 큰 목적은 이런 디스크 I/O를 줄이는 것이다. 

![image](https://user-images.githubusercontent.com/31242766/202230878-c0424a0b-e33c-4790-bd51-6b6860fbc506.png)

만약 crew 테이블 a와 d 사이의 닉네임을 가진 백엔드 크루를 조회한다고 가정해보자. 그리고 원활한 조회를 위해 nickname과 track에 복합 인덱스를 추가하자.
```sql
SELECT *
FROM crew
WHERE nickname BETWEEN 'a' AND 'd' AND track = 'BACKEND'
```
```sql
ALTER TABLE crew ADD INDEX idx_crew_nickname_track (nickname, track);
```

옵티마이저는 전체 데이터의 20 ~ 25% 이상을 조회하는 경우 인덱스를 통해 조회하는 것보다 데이터 파일을 바로 읽는 것(Full Table Scan)이 효율적이라 판단하여 Full Table Scan을 발생시킨다. 이것을 커버링 인덱스를 통해서 개선할 수 있다. 커버링 인덱스는 인덱스로 설정한 컬럼만 읽어 쿼리를 모두 처리할 수 있는 인덱스이며, 불필요한 디스크 I/O를 줄여 조회 시간을 단축할 수 있다. 

![image](https://user-images.githubusercontent.com/31242766/202231725-01d296c4-c6d6-4499-96ac-70b4357bbd7d.png)

모든 컬럼을 조회하던 쿼리에서 nickname과 track으로 생성한 복합 인덱스에 대한 쿼리를 개선하면 추가적인 데이터 파일을 읽지 않고 인덱스만 읽기 때문에 불필요한 디스크 I/O 시간을 단축시킬 수 있다.

![image](https://user-images.githubusercontent.com/31242766/202232332-3d378005-184a-4398-8ae6-4b24d2e10efb.png)

![image](https://user-images.githubusercontent.com/31242766/202232652-8ee7b52b-b831-436a-8cbf-aa775df13e3b.png)

다시 한 번 실행 계획을 살펴보면 type에 Index Range Scan이 발생한 것을 알 수 있고 커버링 인덱스를 타게 되면 Etra 컬럼에 Using Index가 표시되는 것을 알 수 있다. 

![image](https://user-images.githubusercontent.com/31242766/202232971-9557a537-b9e9-49c8-b999-c0ad7a16f77d.png)

![image](https://user-images.githubusercontent.com/31242766/202233299-6b84b785-fbf8-4352-b6bd-3aca88d0cc87.png)

그런데, 만약 프라이머리 키인 id와 함께 조회하게 된다면 어떻게 될까? 이전과 동일하게 커버링 인덱스를 활용한다. 프라이머리키를 복합 인덱스를 설정하지도 않았는데 같은 결과값이 나오는 이유는 InnoDB 세컨터리 인덱스의 특수한 구조 덕분이다. 리프 노드에는 실제 레코드 주소가 아닌 클러스터드 인덱스가 걸린 PK를 주소로 가지고 있기 때문이다. 
```sql
SELECT id, nickname, track
FROM crew
WHERE nickname BETWEEN 'a' AND 'd' AND track = 'BACKEND';
```
![image](https://user-images.githubusercontent.com/31242766/202233677-35f9e844-e5c9-4207-bed3-9da075e5e01e.png)

![image](https://user-images.githubusercontent.com/31242766/202234094-052f2d4b-7b07-4a37-974e-f4916b9752c1.png)

## 인덱스 적용 사례4
### 인덱스 컨디션 푸시다운
한 명의 크루는 N개의 학습 로그를 작성할 수 있다고 하자. 학습 로그에는 type이라는 컬럼이 존재하는데 share, question 등 학습 로그의 목적을 명시하는 컬럼이다. 비즈니스 로직상 type을 기준으로 조회가 많이 일어난다고 가정하고 type 기준으로 조회를 위해서 인덱스를 생성해주었다고 하자.

![image](https://user-images.githubusercontent.com/31242766/202234278-69f4457e-6bd2-4502-a08c-f7d5193700e8.png)

```sql
ALTER TABLE study_log ADD INDEX idx_study_log_type (type);
```

QUESTION 목적을 가진 2022-10-07 ~ 2022-10-13 사이 학습 로그를 조회한다고 해보자. 실행 계획을 살펴보면 생성했던 인덱스가 key 값으로 활용되어 인덱스를 탄 것을 확인할 수 있다. 언뜻 보면 인덱스가 잘 적용된 것처럼 보인다. 하지만 여기서 Extra 컬럼의 `Using where`을 확인해보자. Extra 컬럼에는 쿼리의 실행 계획에서 성능에 관련된 중요한 내용이 표시된다. 내부적인 처리 알고리즘에 대해 조금 더 깊은 내용을 포함한다. `Using where`은 InnoDB 스토리지 엔진을 통해 테이블에서 행을 가져온 뒤, MySQL 엔진에서 추가적인 체크 조건을 활용하여 행의 범위를 축소한 것이다. 
```sql
SELECT *
FROM study_log
WHERE type = 'QUESTION'
AND created_at BETWEEN '2022-10-07' 00:00' AND '2022-10-13 00:00';
```
![image](https://user-images.githubusercontent.com/31242766/202235089-a17efe7a-baab-43e6-892a-d296fc781bad.png)

생성했던 type을 기반으로 생성된 인덱스를 통해서 InnoDB 스토리지 엔진이 조건을 활용해 인덱스 필터링을 거치고 디스크 파일에서 500612개의 데이터를 MySQL 엔진으로 전달한다. MySQL 엔진은 인덱스로 걸지 않은 생성일(created_at)을 기반으로 체크 조건을 통해서 9061개 데이터를 필터링해서 사용자에게 전달해준다. 결국 InnoDB 스토리지 엔진은 불필요하게 너무 많은 데이터를 디스크에서 읽어버린 셈이 되었다.

![image](https://user-images.githubusercontent.com/31242766/202235872-6156a462-d160-42da-8035-a1da06eef65f.png)

이것을 복합 인덱스를 통해서 개선이 가능하다. WHERE 조건에서 사용하고 있는 type과 생성일(created_at)을 기반으로 복합 인덱스를 생성해주었다. 다시 확인해보면 방금 생성했던 복합 인덱스를 key로 활용해서 Index Range Scan이 발생한 것을 알 수 있다. 추가적으로 Extra 컬럼의 Using index condition이 표시된 것을 확인할 수 있는데 `Extra 컬럼`의 `Using Index Condition`은 `인덱스 컨디션 푸시다운`으로 인해 표시된다. `인덱스 컨디션 푸시다운(ICP, Index Condition Pushdown)`이란, MySQL이 `인덱스`를 사용하여 테이블에서 행을 검색하는 경우의 최적화를 의미한다. `ICP`를 활성화하고 인덱스의 컬럼만 사용하여 Where 조건의 일부를 평가할 수 있는 경우 MySQL 엔진은 Where 조건 부분을 스토리지 엔진으로 푸시한다. ICP는 최신 버전의 MySQL을 사용하고 있으면 활성화되어 있는 옵션이다.
```SQL
ALTER TABLE study_log ADD INDEX idx_study_log_type_created_at (type, created_at);
```
![image](https://user-images.githubusercontent.com/31242766/202236894-0b866486-796e-4927-9688-9bff40278ec8.png)

![image](https://user-images.githubusercontent.com/31242766/202238072-d11bd10c-d1cc-4d4b-a3bf-4aab6d3df8e2.png)

단순히 type만 보고 이 실행 계획이 인덱스를 탔다 안 탔다 판단하는 것보다 Extra 컬럼까지 함께 고려해서 인덱스가 적절히 탔는지 함께 고려해줘야 한다. 

![image](https://user-images.githubusercontent.com/31242766/202238210-f801b0bb-1c7e-46fe-be2c-4c290b80e63c.png)

## 더 배울점
- 인덱스 스킵 스캔
- 루스 인덱스 스캔
- 유니크 인덱스
- 전문 검색 인덱스
- 옵티마이저     
...

## 출처
https://www.youtube.com/watch?v=nvnl9YgnON8&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech



