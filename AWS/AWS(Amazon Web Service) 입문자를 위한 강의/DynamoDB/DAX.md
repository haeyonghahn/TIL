# DAX
- 클러스터 In-memory 캐시
- 10배 이상의 속도 향상
- 읽기 요청만 해당사항 (X 쓰기요청)
- Ex) Black Friday날 쇼핑 웹사이트 운영 (수많은 읽기 요청 예상) 

## DAX 원리
- DAX 캐싱 시스템 ➔ 테이블에 데이터 삽입 & 업데이트시 DAX에도 반영
- 읽기 요청에 맞는 데이터가 DAX에 들어있을시 DAX에서 데이터 즉시 반환 (Cache Hit) <-> (Cache Miss)

## DAX의 단점
- 쓰기 요청이 많은 어플리케이션에서는 부적절함
- 읽기 요청이 많지 않은 어플리케이션에서 부적절함
- 아직 모든 지역에서 제공하지 않음

![image](https://user-images.githubusercontent.com/31242766/219366236-da16b79b-0791-43f4-ac2f-611a642be47f.png)
