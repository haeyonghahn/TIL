# Rabbit MQ 설치
## Windows 10
### Erlang 설치
[Erlang 다운로드](https://www.erlang.org/downloads)

![image](https://user-images.githubusercontent.com/31242766/196221294-86577870-2de1-473c-9f69-b7fe2f401c25.png)

### RabbitMQ 설치
[RabbitMQ 다운로드](https://www.rabbitmq.com/install-windows.html)

![image](https://user-images.githubusercontent.com/31242766/196222482-54e222ee-779e-4eee-8cf9-087ec132d7a0.png)

#### 시스템 환경변수 추가

![image](https://user-images.githubusercontent.com/31242766/196224628-1d122385-5e76-4ea2-a847-d94c3c223e65.png)

### RabbitMQ Management Plugin 설치
웹 상에서의 관리/확인을 위해 rabbitMQ Management 를 활성화한다.

[Management Plugin 다운로드](https://www.rabbitmq.com/management.html)

![image](https://user-images.githubusercontent.com/31242766/196223691-28d8d528-c8ac-4b3a-9572-0e203c9a2d5e.png)

![image](https://user-images.githubusercontent.com/31242766/196224733-b3827996-cdc8-423b-abc2-4521ef77f15f.png)

### 서비스 재시작
![image](https://user-images.githubusercontent.com/31242766/196227610-944bcd51-5a2b-4d55-9899-60f855781469.png)

![image](https://user-images.githubusercontent.com/31242766/196227438-90ff0c31-0ea8-401a-907a-a54a8c197a54.png)

### 서버 접속
설정 완료 후 `localhost:15672` 로 접속하여 `id: guest / password : guest` 로 접속한다.

![image](https://user-images.githubusercontent.com/31242766/196228726-87038144-252f-42dc-b015-22352d29abc5.png)

### 계정 설정
외부에서 접속할 수 있는 계정을 설정해 준다. `guest / guest` 로 사용하는 계정이 기본적으로 활성화되어있긴 하지만 해당 계정은 로컬호스트로만 접속이 가능하기 때문에 외부에서 
접속하려면 계정을 새로 설정해야 한다. `RabbitMQ Command Prompt` 를 실행 한 후 계정을 생성한다.
```cmd
rabbitmqctl add_user [ID] [PASSWORD]
rabbitmqctl set_user_tags [ID] administrator <- 관리자 권한을 부여한다.
rabbitmqctl set_permissions -p / [ID] ".*" ".*" ".*"
```

## 참고
https://chanzu.tistory.com/m/105     
https://afsdzvcx123.tistory.com/m/entry/RabbitMQ-RabbitMQ-%EC%84%A4%EC%B9%98
