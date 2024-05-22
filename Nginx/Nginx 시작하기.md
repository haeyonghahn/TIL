# Nginx
### nginx 설치
Amazon Linux에 Nginx를 설치하는 방법을 단계별로 설명. Amazon Linux 2를 기준으로 설명한다.

#### 1. 시스템 업데이트
```linux
sudo yum update -y
```

#### 2. EPEL(Extra Packages for Enterprise Linux) 리포지토리 설치
EPEL 리포지토리에는 Amazon Linux의 기본 리포지토리에 없는 추가 패키지가 포함되어 있다. Nginx를 설치하려면 EPEL 리포지토리를 활성화해야 한다.
```linux
sudo amazon-linux-extras install epel -y
```

#### 3. Nginx 설치
```linux
sudo yum install nginx -y
```

#### 4. Nginx 시작 및 자동 시작 설정
Nginx를 시작하고, 시스템 부팅 시 자동으로 시작되도록 설정한다. 기본적으로 nginx는 서버가 부팅될 때 자동으로 시작한다.
```linux
sudo systemctl start nginx

# 자동 부팅 활성화, 비활성화
sudo systemctl enable nginx
sudo systemctl disable nginx
```

#### 5. Nginx 상태 확인
Nginx가 제대로 설치되고 실행 중인지 확인한다.
```linux
# nginx 버전 확인
nginx -v

nginx version: nginx/1.24.0

# nginx 상태 확인
sudo systemctl status nginx
```

![nginx실행확인](https://github.com/haeyonghahn/TIL/blob/master/Nginx/images/nginx%EC%8B%A4%ED%96%89%ED%99%95%EC%9D%B8.PNG)

#### nginx 서비스 중단하기
```linux
sudo systemctl stop nginx
```

#### nginx 오류 확인하기
```linux
sudo nginx -t
```

#### nginx 재시작하기
```linux
sudo service nginx restart
```

#### 수정된 파일 적용하여 연결을 끊지 않고 재실행
```linux
sudo service nginx reload
```
