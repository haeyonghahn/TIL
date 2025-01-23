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

## Build
```
docker build [OPTIONS] .
```
`-t`, `--tag` : 이미지에 이름과 태그를 지정합니다.    
`-f`, `--file` : Dockerfile의 경로를 지정합니다.    
`--network` : 빌드 시 사용할 네트워크 모드를 지정합니다.    
`--pull` : 베이스 이미지를 항상 다시 다운로드합니다.

### 예시
```
docker build -t my-image:1.0 .
docker build -f ./path/to/Dockerfile .
docker build --network=host .
docker build --pull .
```

## 환경 변수 파일 사용
1. `.env` 파일을 생성합니다.
```
# .env 파일
MY_ENV_VAR=my_value
ANOTHER_VAR=another_value
```
2. Docker 이미지를 빌드하고 `.env` 파일을 사용하여 컨테이너를 실행합니다.
```
# Docker 이미지 빌드
docker build -t my_image .

# 컨테이너 실행 시 .env 파일 사용
docker run --name my_container --env-file .env my_image
```

## Container 
## Stop
```
docker stop <container_name_or_id>
```
## Delete
```
docker rm <container_name_or_id>
```
## 실행 중인 컨테이너를 한 번에 중지하고 삭제하기
```
docker rm -f <container_name_or_id>
```
## 모든 중지된 컨테이너 삭제
```
docker container prune
```
`-f`, `--force` : 자동 확인 옵션으로 사용자 확인 없이 바로 삭제합니다.

## Image
## 특정 이미지 삭제
특정 이미지 ID 또는 이름을 사용하여 삭제합니다.
```
docker rmi <이미지_ID>   # 이미지 ID로 삭제
docker rmi <이미지_이름>  # 이미지 이름으로 삭제
```
## 모든 이미지 삭제
```
docker image prune -a
```
`-a` : 사용하지 않는 모든 이미지 삭제 (태그 없는 이미지 포함).
## 태그 없는 이미지(중간 이미지) 삭제
```
docker image prune
```
## 강제 삭제
```
docker rmi -f <이미지_ID>
```

## Image
## 모든 사용하지 않는 볼륨 삭제
```
docker volume prune
```
`-f`, `--force` : 자동 확인 옵션으로 사용자 확인 없이 바로 삭제합니다.
## 특정 볼륨 삭제
```
docker volume rm <volume_name>
```
## 사용하지 않는 볼륨 확인
```
docker volume ls -f dangling=true
```

## 모든 Docker 리소스와 함께 삭제
사용하지 않는 볼륨과 함께 네트워크, 컨테이너, 이미지 등 다른 리소스도 정리할 수 있습니다.
```
docker system prune --volumes
```
