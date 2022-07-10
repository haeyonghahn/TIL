# Nginx
## EC2에서 nginx 설치
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
```
sudo service nginx start

Redirecting to /bin/systemctl start nginx.service
```
### 실행 확인하기
```linux
sudo systemctl status nginx
```
