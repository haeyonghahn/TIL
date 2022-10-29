# mariaDB 설치
## MacOS
- 설치) $ brew install mariadb
- 시작, 종료, 상태 확인) $ mysql.server start, mysql.server stop, mysql.server.status
- 접속) $ mysql -uroot
- 데이터베이스 생성) mysql> create database mydb;
- Access denied 발생시)
  - $ sudo mysql -u root
  - mysql> use mysql;
  - mysql> select user, host, plugin FROM mysql.user;
  - mysql> set password for 'root'@'localhost'=password('패스워드입력');
  - mysql> flush privileges;

## Windows
