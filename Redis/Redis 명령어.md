# Redis 명령어
## String
문자열, 숫자, serialized object(JSON string) 등 저장
### SET
```redis
set {key} {value} : key, value 를 저장

예시) SET lecture inflearn-redis    
예시) SET inflearn-redis '{"price": 100, "language": "ko"}' 레디스에는 JSON string을 직접 저장할 수도 있다.
```
redis에서는 일반적으로 키를 만들 때 콜론을 이용하여 의미별로 구분해준다.
```redis
'인프런 레디스 강좌의 한국어 버전의 가격이 200 이다'라는 것을 저장하고 싶을 때

SET inflearn-redis:ko:price 200
```
### MSET
```redis
mset {key} {value} [{key} {value} ...] : 여러 개의 key, value 를 한번에 저장

예시) MSET price 100 language ko
```
### MGET
```redis
mget {key} [{key} ...] : 여러 개의 key 에 해당하는 value 를 한번에 가져옴

예시) MGET lecture price language
```
### INCR
```redis
INCR key : incr 명령어는 increase의 약자로 숫자형 스트링 값을 1 올릴 때 사용됨

예시) INCR price
```
### INCRBY
```redis
INCRBY key value : increaseby는 숫자형 스트링의 값에 특정 값을 더할 때 사용됨

예시) INCRBY price 10
```
## Lists
String을 Linked List로 저장 -> push / pop에 최적화 O(1)     
Queue(FIFO) / Stack(FILO) 구현에 사용    
![image](https://github.com/haeyonghahn/TIL/assets/31242766/8f290084-7251-45f3-8c36-4db7fd46509a)
```redis

```
## Set
Redis 에서는 Set 에 포함된 값들을 멤버라고 표현한다. 여러 멤버가 모여 집합 (Set) 을 구성한다.    
```redis
sadd {key} {member} [{member} ...]
key 에 새로운 멤버들을 추가. key 가 없으면 새로 만듬
```
```redis
smembers {key}
key 에 설정된 모든 멤버 반환
```
```redis
srem {key} {member [{member} ...]}
key 에 포함된 멤버들 삭제. 없는 멤버 입력하면 무시됨
```
```redis
scard {key}
key 에 저장된 멤버 수를 반환
```
```redis
sismember {key} {member}
member 가 해당 key 에 포함되는지 검사
```
## Hash
Hash 자체를 나타내는 key 와 해당 key 에 포함된 field 까지 사용해서 값을 조회 및 저장할 수 있다.
```redis
hset {key} {field} {value} [{field} {value} ...]
key 를 이름으로 한 Hash 자료 구조에 field 와 value 값을 저장
```
```redis
hget {key} {field}
key Hash 값에 포함된 field 의 value 를 가져옴
```
```redis
hdel {key} {field} [{field} ...]
field 값으로 데이터 삭제
```
```redis
hlen {key}
Hash 가 갖고 있는 field 갯수 반환
```
```redis
hkeys {key}
Hash 가 갖고 있는 모든 field 출력
```
```redis
hvals {key}
Hash 가 갖고 있는 모든 value 출력
```
```redis
hgetall {key}
Hash 가 갖고 있는 모든 field 와 value 출력
```
## 조회
```redis
keys * : 
현재 저장된 키값들을 모두 확인 (부하가 심한 명령어라 운영중인 서비스에선 절대 사용하면 안됨)
```
```redis
ttl {key} : key 의 만료 시간을 초 단위로 보여줌 (-1 은 만료시간 없음, -2 는 데이터 없음)
```
```redis
pttl {key} : key 의 만료 시간을 밀리초 단위로 보여줌
```
```redis
type {key} : 해당 key 의 value 타입 확인
```
## 삭제
```redis
del {key} [{key} ...] : 해당 key 들을 삭제
```
## 수정
```redis
rename {key} {newKey} : key 이름 변경
expire {key} {seconds} : 해당 키 값의 만료 시간 설정
```
## 기타
```redis
randomkey : 랜덤한 key 반환
```
```redis
ping : 연결 여부 확인 ("ping" 만 입력하면 "PONG" 이라는 응답이 옴)
```
```redis
dbsize : 현재 사용중인 DB 의 key 의 갯수 리턴
```
```redis
flushall : 레디스 서버의 모든 데이터 삭제
```
```redis
flushdb : 현재 사용중인 DB 의 모든 데이터 삭제
```
```redis
info : redis 서버 설정 상태 조회

예시) info replication
```
