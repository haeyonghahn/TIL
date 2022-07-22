# Redis 설치
## Amazon EC2 서버에 Redis 설치하기
```linux
# make 하기 위핸 gcc 다운
[ec2-user@ip-xx.xx.xxx ~]$ sudo yum install -y gcc

# redis-cli 설치 및 make
[ec2-user@ip-xx.xx.xxx ~]$ wget http://download.redis.io/redis-stable.tar.gz && tar xvzf redis-stable.tar.gz && cd redis-stable && make

# redis-cli를 bin에 추가해 어느 위치서든 사용 가능하게 등록
[ec2-user@ip-xx.xx.xxx ~]$ sudo cp src/redis-cli /usr/bin/

# 접속 테스트
[ec2-user@ip-xx.xx.xxx ~]$ redis-cli -h ec2-xx-xx-xxx.ap-northeast-2.compute.amazonaws.com
```
~~## 디렉토리 생성 및 복사~~
~~$ sudo mkdir /etc/redis~~    
~~$ sudo mkdir /var/lib/redis~~        
~~$ sudo cp src/redis-server src/redis-cli /usr/local/bin/~~   
~~$ sudo cp redis.conf /etc/redis/~~   

~~## /etc/redis.conf 수정~~     
~~daemonize yes~~   
~~bind 0.0.0.0                      // 모든 IP 접근허용처리~~     
~~dir /var/lib/redis~~      
~~logfile "/var/log/redis_6379.log"~~      

~~## 자동 실행을 위한 설정~~    
~~$ cd /tmp~~    
~~$ wget https://raw.github.com/saxenap/install-redis-amazon-linux-centos/master/redis-server~~
 
~~$ sudo mv redis-server /etc/init.d~~
~~$ sudo chmod 755 /etc/init.d/redis-server~~

~~$ sudo chkconfig --add redis-server~~    
~~$ sudo chkconfig --level 345 redis-server on~~

~~$ sudo service redis-server stop              // 실행중단~~    
~~$ sudo service redis-server start             // 실행~~   
~~$ systemctl status redis-server.service       // 상태확인~~   
~~$ sudo systemctl restart redis-server.service // 재실행~~   

## 백그라운드 실행 설정
```linux
vi redis.conf 
[...]
daemonize no <- yes로 변경
[...]
```
## redis-server 실행
```linux
[ec2-user@ip-xxx.xx.xxx redis-stable]$ src/redis-server ./redis.conf
```

## redis 통신 확인
```linux
$ ps aux | grep redis
```
## 포트 죽이기 
```linux
kill -9 8821
```

## 출처
https://uwostudy.tistory.com/73?category=328168    
https://mygumi.tistory.com/133    
https://www.codegrepper.com/code-examples/whatever/Could+not+create+server+TCP+listening+socket+%2A%3A6379%3A+bind%3A+Address+already+in+use
