# Docker 명령어
## Network
```
docker network create : 새로운 네트워크를 생성합니다.
docker network ls : 현재 존재하는 네트워크 목록을 출력합니다.
docker network inspect : 특정 네트워크의 세부 정보를 출력합니다.
docker network rm : 특정 네트워크를 삭제합니다.
docker network connect : 컨테이너를 네트워크에 연결합니다.
docker network disconnect : 컨테이너를 네트워크에서 분리합니다.
docker network prune : 사용하지 않는 모든 네트워크를 삭제합니다.
```

## Network Create
```
docker network create [OPTIONS] NETWORK
```
`--driver` : 네트워크 드라이버를 지정합니다. 기본값은 bridge입니다. 다른 옵션으로 overlay, macvlan 등이 있습니다.   
`--subnet` : 네트워크의 서브넷을 지정합니다.   
`--gateway` : 네트워크의 게이트웨이를 지정합니다.   
`--ip-range` : 네트워크의 IP 주소 범위를 지정합니다.   
`--attachable` : 컨테이너가 독립적으로 네트워크에 연결될 수 있도록 합니다.   

### 예시
```
docker network create --driver bridge my_bridge_network
docker network create --driver overlay --subnet 10.0.0.0/24 my_overlay_network
```

## Network ls
```
docker network ls
```
`--filter` : 특정 조건에 맞는 네트워크만 필터링하여 출력합니다.

### 예시
```
docker network ls --filter driver=bridge
```
