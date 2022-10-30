# Kafka Connect
- kafka Connect라는 것은 Data를 자유롭게 Import/Export 가능
- 코드 없이 Configuration으로 데이터를 이동
- Standalone mode, Distribution mode 지원
  - RESTful API 통해 지원
  - Stream 또는 Batch 형태로 전송 가능
  - 커스텀 Connector를 통해 다양한 Plugin 제공(File, S3, Hive, Mysql, ...)
  
Kafka Cluster 상태에서 데이터를 가져오는 쪽(`Kafka Connect Source`)으로 특정한 리스소에서 데이터를 가져와 Kafka Cluster에 데이터를 저장하고 데이터를 보내는 쪽(`Kafka Connect Sink`)을 통해 다른 `Traget System`에 데이터를 Export를 할 수 있다. 예를 들어, 특정한 DB에 어떠한 자료가 저장이 되었다면, 우리가 복제 작업을 위해서. 또는 mySQL에 전달된 데이터를 동시에 Oracle에 전달하고자 할 때에도 `Kafka Connect`을 이용해서 작업을 할 수 있다.

![image](https://user-images.githubusercontent.com/31242766/198813585-523cca29-5fe4-45b3-9a08-68c4241d20e0.png)

## MacOS 설치
### Kafka Connect 설치
```linux
curl -O http://packages.confluent.io/archive/5.5/confluent-community-5.5.2-2.12.tar.gz
curl -O http://packages.confluent.io/archive/6.1/confluent-community-6.1.0.tar.gz
tar xvf confluent-community-6.1.0.tar.gz
cd  $KAFKA_CONNECT_HOME
```
### Kafka Connect 실행
```linux
./bin/connect-distributed ./etc/kafka/connect-distributed.properties
```
### Topic 목록 확인
```linux
./bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```
### JDBC Connector 설치
https://docs.confluent.io/5.5.1/connect/kafka-connect-jdbc/index.html     
confluentinc-kafka-connect-jdbc-10.0.1.zip 

### etc/kafka/connect-distributed.properties 파일 마지막에 아래 plugin 정보 추가
```properties
plugin.path=[confluentinc-kafka-connect-jdbc-10.0.1 폴더]
```

### JdbcSourceConnector에서 MariaDB 사용하기 위해 mariadb 드라이버 복사
./share/java/kafka/ 폴더에 mariadb-java-client-2.7.2.jar  파일 복사

## Windows 설치
### Kafka Connect 설치
```powershell
curl -O http://packages.confluent.io/archive/5.5/confluent-community-5.5.2-2.12.tar.gz
curl -O http://packages.confluent.io/archive/6.1/confluent-community-6.1.0.tar.gz
tar xvf confluent-community-6.1.0.tar.gz
cd  $KAFKA_CONNECT_HOME
```
### Kafka Connect 실행
```powershell
.\bin\windows\connect-distributed.bat .\etc\kafka\connect-distributed.properties
```
- 실행 시 아래와 같은 오류 발생하면, binary 파일 대신 source 파일을 다운로드 받은 것인지 확인
  - __Classpath is empty. Please build the project e.g. by running 'gradlew jarAll'__
  
  ![image](https://user-images.githubusercontent.com/31242766/198863313-217cd01b-e2e7-4b72-a4bc-e6eaa4b9b21c.png)
- .\bin\windows\kafka-run-class.bat 파일에서 
  - __rem Classpath addition for core 부분을 찾아서, 그 위에 코드를 삽입__
  
  ![image](https://user-images.githubusercontent.com/31242766/198863432-8b405a6b-b350-453c-a58a-c24cfb7b8690.png)
- .\etc\kafka\connect-distributed.properties 파일 마지막에 아래 plugin 정보 추가
