# Rabbit MQ
## 목차
* **[메시지 큐(Message Queue)란?](#메시지-큐(Message-Queue)란?)**
* **[메시지 큐의 장점](#메시지-큐의-장점)**
* **[메시지 큐 사용처](#메시지-큐-사용처)**
* **[JMS vs AMQP](#JMS-vs-AMQP)**
* **[RabbitMQ](#RabbitMQ)**

## 메시지 큐(Message Queue)란?
`메시지 지향 미들웨어(Meesage Oriented Middleware: MOM)은 비동기 메시지를 사용하는 다른 응용 프로그램 사이에서 데이터 송수신을 의미한다.` MOM을 구현한 시스템을 메시지 큐(MessageQueue: MQ)라 한다. 

`프로그래밍에서 MQ는 프로세스 또는 프로그램 인스턴스가 데이터를 서로 교환할때 사용하는 방법이다.` 이때 데이터를 교환할 때 시스템이 관리하는 메시지 큐를 이용하는 것이 특징이다. 이렇게 서로 다른 프로세스나 프로그램 사이에 메시지를 교환할 때 AMQP(Advanced Message Queuing Protocol)을 이용한다. AMQP는 메시지 지향 미들웨어를 위한 open standard application layer protocol이다. `AMQP를 이용하면 다른 벤더 사이에 메세지를 전송하는 것이 가능한데 JMS(Java Message Service)가 API를 제공하는 것과 달리 AMQP는 wire-protocol을 제공하는데 이는 octet stream을 이용해서 다른 네트워크 사이에 데이터를 전송할 수 있는 포맷으로 이를 사용하게 된다.`

## 메시지 큐의 장점
- `비동기(Asynchronous)`: Queue에 넣기 때문에 나중에 처리할 수 있다.
- `비동조(Decoupling)`: 애츨리케이션과 분리할 수 있다.
- `탄력성(Resilience)`: 일부가 실패 시 전체에 영향을 받지 않는다.
- `과잉(Redundancy)`: 실패할 경우 재실행 가능하다.
- `보증(Guarantees)`: 작업이 처리된걸 확인할 수 있다.
- `확장성(Scalable)`: 다수의 프로세스들이 큐에 메시지를 보낼 수 있다.

Message Queueing은 대용량 데이터를 처리하기 위한 배치 작업이나, 채팅 서비스, 비동기 데이터를 처리할 때 사용한다. 프로세스 단위로 처리하는 웹 요청이나 일반적인 프로그램을 만들어서 사용하는데 사용자가 많아지거나 데이터가 많아지면 요청에 대한 응답을 기다리는 수가 증가하다가 나중에는 대기 시간이 지연되어서 서비스가 정상적으로 되지 못하는 상황이 오기 때문에 기존에 분산되어 있던 데이터 처리를 한 곳에 집중하면서 메세지 브로커를 두어서 필요한 프로그램에 작업을 분산시키는 방법을 하는 것이 그 목적이다. 또한, 해당하는 요청을 다른 API에게 위임하고 빠른 응답을 할 때 많이 사용한다. MQ를 사용하면 애플리케이션간에 결합도를 낮출 수 있다는 장점도 가진다.

## 메시지 큐 사용처
- 다른 곳의 API로 부터 데이터 송수신이 가능하다.
- 다양한 애플리케이션에서 비동기 통신을 할 수 있다.
- 이메일 발송 및 문서 업로드가 가능하다.
- 많은 양의 프로세스들을 처리할 수 있다.
  - 요청을 많은 사용자에게 전달할 때
  - 요청에 대한 처리시간이 길 때
  - 많은 작업이 요청되어 처리를 해야할 때

## JMS vs AMQP
- AMQP는 ISO 응용 계층의 MOM 표준이다.
- JMS는 MOM을 자바에서 지원하는 표준 API이다. (JMS와 AMQP는 다른 개념이다)
- JMS는 다른 자바 애플리케이션들끼리 통신이 가능하지만 다른 MOM의 통신은 불가능 하다. (AMQP, SMTP 등)
- ActiveMQ의 JMS라이브러리를 사용한 자바 애플리케이션들끼리 통신이 가능하긴 합니다. 하지만 다른 자바 애플리케이션(ActiveMQ를 사용안함)의 JMS와는 통  신할 수 없다.
- AMQP는 프로토콜만 맞다면 다른 AMQP를 사용한 애플리케이션끼리 통신이 가능합니다. SMTP와도 통신이 가능하다.
- JMS 라이브러리는 AMQP를 지원하지는 않는다.

## RabbitMQ
![image](https://user-images.githubusercontent.com/31242766/203707979-2db0f1cd-ccc0-4cde-83d5-c759553a28a0.png)
- `Producer` : 메세지를 생성하고 발송하는 주체이다. 이 메세지는 Queue에 저장이 되는데, 주의할 점은 Producer는 Queue에 직접 접근하지 않고, 항상 Exchange를 통해 접근하게 된다.
- `Exchange` : Producer들에게서 전달받은 메세지들을 어떤 Queue들에게 발송할지를 결정하는 객체이다. Exchange는 네 가지 타입이 있으며, 일종의 라우터 개념이다.
![image](https://user-images.githubusercontent.com/31242766/203708303-fe30d78c-7e1b-439c-aa57-23f17bd0b908.png)
  - Name: Exchange 이름
  - Type: 메시지 전달 방식
    |타입|설명|특징|
    |------|---|---|
    |Direct|Routing key가 정확히 일치하는 Queue에 메세지 전송|Unicast|
    |Fanout|Routing key 패턴이 일치하는 Queue에 메세지 전송|Multicast|
    |Topic|[key:value]로 이루어진 header 값을 기준으로 일치하는 Queue에 메세지 전송|Multicast|
    |Headers|해당 Exchange에 등록된 모든 Queue에 메세지 전송. Headers 타입의 경우, 모든 header가 일치할 때, 하나라도 일치할 때 메세지 전송 등의 옵션을 줄 수 있다.|Broadcast|
    - Direct Exchange    
      Direct Exchange는 라우팅 키를 이용하여 메세지를 라우팅 하는데, 하나의 큐에 여러 개의 라우팅 키를 지정할 수 있다.
      ![image](https://user-images.githubusercontent.com/31242766/203713629-1136f5c2-2ba4-4d8c-96c2-90d68d83183d.png)   
      라우팅 키를 이용하여 메시지를 전달할 때 정확히 일치하는 Queue에만 전송한다.
      하나의 Queue에 여러 라우팅 키를 지정할 수 있고, 여러 Queue에 같은 라우팅 키를 지정할 수도 있다.
      Default Exchange는 이름이 없는 Direct Exchange이다. 전달된 목적 Queue 이름과 동일한 라우팅 키를 부여한다.
    - Fanout Exchange     
      exchange에 등록된 모든 queue에 메세지를 전송합니다.
      ![image](https://user-images.githubusercontent.com/31242766/203713968-701b5d71-b87b-4234-92fa-33044a4726e6.png)
    - Topic Exchange      
      여러 Consumer에서 메시지 형태에 따라 선택적으로 수신해야하는 경우 등등 다양한 패턴 구현에 활용될 수 있다.
      ![image](https://user-images.githubusercontent.com/31242766/203714669-38efd3dc-e008-42dd-8b64-4f0e1c514e87.png)
      위 그림의 경우에는 라우팅 키가 정확히 일치하지 않아 binding되지 않은 경우이다.
      ![image](https://user-images.githubusercontent.com/31242766/203714742-600c4a75-7f7f-46fd-8831-e1a5232d6320.png)
      위 그림의 경우에는 "animal.rabbit"이 "animal.* "와 "#" 모두에 일치하기 때문에 모든 Queue에 전송되는 경우이다.
      (#은 0개 이상의 단어를 대체한다.)
    - Headers Exchange      
      Headers Exchange는 Topic Exchange와 유사하지만 라우팅을 위해 header를 쓴다는 차이점이 있다.
      ![image](https://user-images.githubusercontent.com/31242766/203715113-487f5ce6-f190-4b26-aea7-35a254486a90.png)
      Topic과 유사한 방법이지만 라우팅을 위해 header를 사용한다는 점에서 차이가 있다.
      producer에서 정의된 header의 key-value 쌍과 consumer에서 정의된 argument의 key-value 쌍이 일치하면 binding된다.
      binding key 만을 사용하는 것보다 더 다양한 속성을 사용할 수 있다.
      이 타입을 사용하면 binding key는 무시되고, 바인딩 시 지정된 값과 헤더 값이 같은 경우에만 일치하는 것으로 간주된다.      
      x-match의 값에 따라 다르게 동작한다.     
      |key|value|설명|
      |------|---|---|
      |x-match|all|header의 모든 key-value 쌍 값과 argument의 모든 key-value쌍 값이 일치할 때 binding
      
  - Durability: 브로커가 재시작될 때 남아있는지 여부
  - Durable: 브로커가 재시작되어도 디스크에 저장되어 남아있음
  - Transient: 브로커가 재시작되면 사라짐
  - Auto-delete: 마지막 Queue 연결이 해제되면 삭제
- `Queue` : 

## 출처
https://12bme.tistory.com/176     
https://sugerent.tistory.com/644    
https://velog.io/@sdb016/RabbitMQ-%EA%B8%B0%EC%B4%88-%EA%B0%9C%EB%85%90     
https://blog.dudaji.com/general/2020/05/25/rabbitmq.html

