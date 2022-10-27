# Kafka 사용하기
## Kafka Client
- Spring 을 기반으로 개발을 진행한다면 Kafka와 데이터를 주고받기 위해 사용하는 [Java Library를 다운](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients)받아야한다.
- Producer, Consumer, Admin, Stream 등 Kafka 관련 API를 제공해준다.
- C/C++, Node.js, Python, .NET 등 다양한 시스템에서 사용할 수 있는 라이브러리를 제공해준다.
  - https://cwiki.apache.org/confluence/display/KAFKA/Clients

![image](https://user-images.githubusercontent.com/31242766/197896542-e5edc207-0fa0-47e8-9762-30a34ac864f0.png)

## Kafka 서버 기동
`kafka`가 설치된 경로로 이동하여 아래와 같은 명령어를 실행한다. (window 를 사용하고 있다.) 
### Zookeeper 서버 기동
```console
.\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```
### kafka 서버 기동
```console
.\bin\windows\kafka-server-start.bat .\config\server.properties
```
### topic 기동
#### topic 리스트 확인
```console
.\bin\windows\kafka-topics.bat --bootstrap-server localhost:9092 --list
```
#### topic 생성하기
```console
.\bin\windows\kafka-topics.bat --bootstrap-server localhost:9092 --create --topic quickstart-events --partitions 1
```
#### topic 상세 정보
```console
.\bin\windows\kafka-topics.bat --bootstrap-server localhost:9092 --describe --topic quickstart-events
```

### producer 기동
```console
.\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic quickstart-events
```

### consumer 기동
```console
.\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic quickstart-events --from-beginning
```
