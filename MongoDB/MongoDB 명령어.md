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
