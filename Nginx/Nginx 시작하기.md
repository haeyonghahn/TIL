# Nginx
### nginx 설치
`amazon-linux-2`의 경우 `yum`을 활용하여 `nginx`의 설치가 지원되지 않는다. 그렇기 때문에 `amazon-linux-extras`를 활용하여 nginx를 설치해야 한다.
```linux
sudo amazon-linux-extras install -y nginx1
```
### 버전 확인
```linux
nginx -v

nginx version: nginx/1.20.0
```
### nginx 서비스 시작하기
```linux
sudo service nginx start

Redirecting to /bin/systemctl start nginx.service
```
### 실행 확인하기
```linux
sudo systemctl status nginx
```
![nginx실행확인](https://github.com/haeyonghahn/TIL/blob/master/Nginx/images/nginx%EC%8B%A4%ED%96%89%ED%99%95%EC%9D%B8.PNG)

### nginx 서비스 중단하기
```linux
sudo systemctl stop nginx
```
### nginx 오류 확인하기
```linux
sudo nginx -t
```
### nginx 재시작하기
```linux
sudo service nginx restart
```
### 수정된 파일 적용하여 연결을 끊지 않고 재실행
```linux
sudo service nginx reload
```
### 자동 시작 비활성화, 활성화
```linux
기본적으로 nginx는 서버가 부팅될 때 자동으로 시작한다.

sudo service disable nginx
sudo service enable nginx
```
### 참고 
https://jojoldu.tistory.com/441
