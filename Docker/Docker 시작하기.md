# Docker
### docker 설치
Amazon Linux에 Docker를 설치하는 방법은 다음과 같습니다. 이 과정은 Amazon Linux 2를 기준으로 설명합니다.

#### 1. 시스템 업데이트
```linux
sudo yum update -y
```

#### 2. Docker 설치
```linux
sudo amazon-linux-extras install docker (Amazon Linux 2)
sudo yum install -y docker (Amazon Linux 2023)
```

#### 3. Docker 서비스 시작 및 부팅 시 자동 시작 설정
Docker 서비스를 시작하고, 부팅 시 자동으로 시작되도록 설정합니다.
```linux
sudo service docker start
sudo systemctl enable docker
```

#### 4. 현재 사용자에게 Docker 권한 부여
현재 사용자가 Docker 명령을 사용할 수 있도록 docker 그룹에 추가합니다. 이렇게 하면 sudo 없이 Docker 명령을 실행할 수 있습니다.
```linux
sudo usermod -a -G docker ec2-user
```

### Docker Compose 설치
```linux
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
설치 후 버전을 확인하여 제대로 설치되었는지 확인할 수 있습니다.
```linux
docker-compose --version
```
