# Kafka 설치
[kafka 다운로드](https://kafka.apache.org/downloads)

![tempsnip](https://user-images.githubusercontent.com/31242766/197892890-a5557352-fd2b-46d5-b08a-94f9dd95b593.png)

## 압축 해제
window 환경에서도 .tgz파일을 압축 해제할 수 있도록 명령어를 제공하고 있다.

![image](https://user-images.githubusercontent.com/31242766/197893372-cbc8093f-5855-4198-9422-5aea9c0337cb.png)

kafka 에서 많이 사용하게 될 폴더는 `bin` `config` 폴더이다. `bin`폴더는 각종 실행 커맨드가 들어가 있는 폴더이며 `config`폴더는 각종 설정 파일이 들어가 있는 폴더이다.

![image](https://user-images.githubusercontent.com/31242766/197893803-e67ae7dd-b5ce-465b-95b2-8856e96362e7.png)

`config`폴더를 보면 `zookeeper.properties`설정하는 파일이 존재하며 apache kafka 서버를 기동할 때 사용되는 `server.properties`파일이 존재한다. apache kafka는 로그를 연동하는 
시스템이나 다른 쪽에서 데이터를 가지고와 보관하고 있다가 다른 쪽의 traget 리소스에 전달시켜줄 수 있는 싱크 기능이 있다. 이러한 해당 설정 파일들이 `config`폴더에 존재한다.

![image](https://user-images.githubusercontent.com/31242766/197894658-bda8d8ce-8e91-4486-a869-0d1c2c2ff00d.png)

`bin`폴더를 보면 macOS 나 linux에서 실행할 수 있는 .sh파일이 존재하며 windows에서 실행할 수 있는 windows 폴더가 따로 존재한다. `zookeeper`실행할 수 있는 start파일이 존재하며 
실행을 종료할 수 있는 stop파일도 존재한다. 또한, `kafka`를 실행할 수 있는 start파일이 존재하며 실행을 종료할 수 있는 stop파일도 존재한다.

![image](https://user-images.githubusercontent.com/31242766/197895398-bec51ac4-a614-4fb9-8d25-5ca7997d0971.png)
