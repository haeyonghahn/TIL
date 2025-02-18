# MongoDB 명령어
## 접속
```
mongosh "mongodb://<username>:<password>@<host>:<port>/?authSource=admin"
```
- `<username>` : MongoDB 사용자 이름
- `<password>` : 사용자 비밀번호
- `<host>` : 서버 주소 (예: localhost 또는 원격 서버 주소)
- `<port>` : MongoDB 포트 (기본값 27017)
- `authSource=admin` : 인증할 데이터베이스 (보통 admin)

## 검색
### 전체 검색
```
db.store.find()
```

### 포맷된 출력
```
db.store.find().pretty()
```

### 조건 검색
#### name이 "Apple Store"인 문서 조회
```
db.store.find({ name: "Apple Store" })
```

### 개수 제한 (limit()) & 건너뛰기 (skip())
```
db.store.find().limit(5)
```
```
db.store.find().skip(10).limit(5)
```

## 수정
### updateOne()으로 특정 도큐먼트 업데이트
특정 조건을 만족하는 첫 번째 도큐먼트의 특정 필드 값을 변경
```
db.collection.updateOne(
   { <조건> },
   { $set: { <필드>: <새로운 값> } }
)
```

#### 예제: users 컬렉션에서 name이 "John"인 도큐먼트의 age 값을 30으로 변경
```
db.users.updateOne(
   { name: "John" },
   { $set: { age: 30 } }
)
```

### findOneAndUpdate()로 변경 후 도큐먼트 반환
업데이트된 도큐먼트를 바로 확인하고 싶다면 findOneAndUpdate()를 사용
```
db.collection.findOneAndUpdate(
   { <조건> },
   { $set: { <필드>: <새로운 값> } },
   { returnDocument: "after" }  // 업데이트 후 결과 반환
)
```

#### 예제: name이 "John"인 도큐먼트의 status를 "active"로 변경 후 결과 반환
```
db.users.findOneAndUpdate(
   { name: "John" },
   { $set: { status: "active" } },
   { returnDocument: "after" }
)
```

### 특정 필드 값 증가 또는 감소 ($inc)
숫자 값을 증가 또는 감소시키려면 $inc 연산자 사용
```
db.collection.updateOne(
   { <조건> },
   { $inc: { <필드>: 증가/감소 값 } }
)
```

#### 예제: John의 points를 +10 증가
```
db.users.updateOne(
   { name: "John" },
   { $inc: { points: 10 } }
)
```

### 특정 필드 삭제 ($unset)
필드(컬럼)를 삭제하려면 $unset 사용
```
db.collection.updateOne(
   { <조건> },
   { $unset: { <필드>: "" } }
)
```

#### 예제: John의 tempField 필드 삭제
```
db.users.updateOne(
   { name: "John" },
   { $unset: { tempField: "" } }
)
```

### 필드명 변경 ($rename)
컬럼명을 변경하려면 $rename 사용
```
db.collection.updateOne(
   { <조건> },
   { $rename: { "oldField": "newField" } }
)
```

#### 예제: fullName 필드를 name으로 변경
```
db.users.updateOne(
   {},
   { $rename: { "fullName": "name" } }
)
```
