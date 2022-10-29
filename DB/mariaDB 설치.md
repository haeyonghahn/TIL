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
[mariaDB 다운로드](https://mariadb.org/) 

![image](https://user-images.githubusercontent.com/31242766/198815771-8d400e75-341e-4b1c-91a5-b72bd5e63d19.png)

- Database 초기화
 - 다운받은 `mariadb-10.5.17-winx64.zip` 파일을 작업 디렉토리로 이동 및 압축 해제
 - 관리자 모드 cmd를 실행 후 데이터베이스 초기화
```console
.\bin\mariadb-install-db.exe 
  --datadir=C:\mariadb-10.5.17-winx64\data 
  --service=mariaDB 
  --port=3306 
  --password=test1357
```
![image](https://user-images.githubusercontent.com/31242766/198815826-3a708900-2924-4f4f-a5dc-80f436acb817.png)

- `mariaDB` 서비스 시작

![image](https://user-images.githubusercontent.com/31242766/198815975-63c22158-1d12-4a46-9222-2272125573d9.png)

- `mariaDB` 접속

![image](https://user-images.githubusercontent.com/31242766/198816316-dcaa9339-cae9-4aea-a936-0452aa53a525.png)
