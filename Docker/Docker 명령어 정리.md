# Docker 명령어 정리
- 컨테이너 실행
```docker
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]

IMAGE : 이미지 이름
TAG : TAG 이름을 명시하지 않으면 기본적으로 'latest'라는 이름이 자동으로 붙는다. 일종의 버전처럼 생각하면 된다.
예시) docker run ubuntu:16.04
```
|옵션|설명|
|----|------------------------|
|-d|detached mode 흔히 말하는 백그라운드 모드|
|-p|호스트(Host PC)와 컨테이너(Guest PC)의 포트를 연결 (포워딩)|
|-v|호스트(Host PC)와 컨테이너(Guest PC)의 디렉토리를 연결 (마운트)|
|-e|컨테이너 내에서 사용할 환경변수 설정|
|--name|컨테이너 이름 설정|
|-rm|프로세스 종료시 컨테이너 자동 제거|
|-it|-i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
|--link|컨테이너 연결 [컨테이너명:별칭]|

- 컨테이너 확인
```docker
docker container ls
```
- 이미지 확인
```docker
docker image ls
```
