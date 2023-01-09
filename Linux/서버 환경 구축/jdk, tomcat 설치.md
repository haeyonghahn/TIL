# jdk, tomcat 설치
### JDK 설치
#### local(windows) cmd에서 가상머신(linux)로 파일 전송
```linux
sftp 사용자계정명(ex. webmaster)@ip주소 입력
put jdk-8u241-linux-x64.tar.gz

root@localhost ~]# tar xvfz jdk-8u241-linux-x64.tar.gz
```
#### 압축 풀기
```linux
root@localhost ~]# tar xvfz jdk-8u241-linux-x64.tar.gz
```
#### 이름 변경
```linux
root@localhost ~]# mv jdk1.8.0_241 jdk1.8
```
#### 파일 이동
```linux
root@localhost ~]# mv jdk1.8 /usr/local/myserver
```
#### 링크 설정
```linux
root@localhost myserver]# ln -s jdk1.8 java
```
#### Java 환경변수 설정
```linux
root@localhost ~]# vi /etc/profile
export JAVA_HOME=/usr/local/myserver/java
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=$JAVA_HOME/lib/tools.jar
```
#### Java 설치 확인
```linux
root@localhost ~]# java -version
root@localhost ~]# javac -version
```

### tomcat 설치
#### tomcat 다운로드
```linux
root@localhost ~]# wget https://mirror.navercorp.com/apache/tomcat/tomcat-8/v8.5.69/bin/apache-tomcat-8.5.69.tar.gz
```
#### 압축 풀기
```linux
root@localhost ~]# tar xvfz apache-tomcat-8.5.69.tar.gz
```
#### 이름 변경
```linux
root@localhost ~]# mv apache-tomcat-8.5.69 tomcat8.5
```
#### 파일 이동
```linux
root@localhost ~]# mv tomcat8.5 /usr/local/myserver
```
#### 링크 설정
```linux
root@localhost myserver]# ln -s tomcat8.5 tomcat
```
#### tomcat 환경변수 설정
```linux
root@localhost ~]# vi /etc/profileroot@localhost ~]# source /etc/profile
export CATALINA_HOME=/usr/local/myserver/tomcat
```
#### tomcat 시작
```linux
root@localhost ~]# /usr/local/myserver/tomcat/bin/catalina.sh start
```
#### 정상 작동 중인지 확인
```linux
root@localhost ~]# ps -ef | grep tomcat
```
#### 방화벽 설정
```linux
root@localhost ~]# vi /etc/sysconfig/iptables 이동
-A INPUT -i eth0 -j ACCEPT 추가 
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT 추가 

root@localhost ~]# /etc/init.d/iptables restart
```
#### 서버 구동 시 바로 tomcat 실행 설정
```linux
root@localhost ~]# cd /etc/init.d
root@localhost ~]# vi tomcat
#!/bin/sh 
# 
# Startup script for Tomcat for HMO
# 
# chkconfig: 35 85 35 
# description: Start Tomcat 
# 
# processname: tomcat 
# 
# Source function library. 

. /etc/rc.d/init.d/functions 

export JAVA_HOME=/usr/local/myserver/java
export CLASSPATH=.:$JAVA_HOME/lib/tools.jar
export CATALINA_HOME=/usr/local/myserver/tomcat
export PATH=$PATH:$JAVA_HOME/bin

case "$1" in 

    start) 

        echo -n "Starting tomcat: " 
        daemon $CATALINA_HOME/bin/startup.sh 
        touch /var/lock/subsys/tomcat
        echo
        ;; 
    stop) 
        echo -n "Shutting down tomcat: " 
        daemon $CATALINA_HOME/bin/shutdown.sh 
        rm -f /var/lock/subsys/tomcat
        echo 
        ;; 
    restart) 
        $0 stop
        sleep 2 
        $0 start 
        ;; 
    *) 
        echo "Usage: $0 {start|stop|restart}" 
        exit 1 
esac 
exit 0
```
#### 권한 주기
```linux
root@localhost init.d]# chmod 755 tomcat
```
#### 부팅시에 자동으로 스크립트 실행을 통해 특정 데몬의 시작 여부를 결정하는 명령어
```linux
root@localhost init.d]# chkconfig --add tomcat
```
#### tomcat 재시작
```linux
root@localhost ~]# /etc/init.d/tomcat restart
```
#### tomcat manager 설정
```linux
root@localhost ~]# cd /usr/local/myserver/tomcat/webapps/manager/META-INF
root@localhost META-INF]# vi context.xml 수정 (아래 코드 주석 처리)
root@localhost ~]# cd /usr/local/douzone/tomcat/conf

<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />`
```
#### root@localhost conf]# vi tomcat-users.xml에 추가
```linux
<role rolename="manager"/> 
<role rolename="manager-gui"/> 
<role rolename="manager-script"/> 
<role rolename="manager-jmx"/> 
<role rolename="manager-status"/> 
<role rolename="admin"/> 
<user username="admin" password="admin" roles="admin,manager,manager-gui,manager-script,manager-jmx, manager-status"/>
```
#### tomcat 재시작
```linux
root@localhost ~]# /etc/init.d/tomcat restart
```
