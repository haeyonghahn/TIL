# Kafka Connect
- kafka Connect라는 것은 Data를 자유롭게 Import/Export 가능
- 코드 없이 Configuration으로 데이터를 이동
- Standalone mode, Distribution mode 지원
  - RESTful API 통해 지원
  - Stream 또는 Batch 형태로 전송 가능
  - 커스텀 Connector를 통해 다양한 Plugin 제공(File, S3, Hive, Mysql, ...)
  
Kafka Cluster 상태에서 데이터를 가져오는 쪽(`Kafka Connect Source`)으로 특정한 리스소에서 데이터를 가져와 Kafka Cluster에 데이터를 저장하고 데이터를 보내는 쪽(`Kafka Connect Sink`)을 통해 다른 `Traget System`에 데이터를 Export를 할 수 있다. 예를 들어, 특정한 DB에 어떠한 자료가 저장이 되었다면, 우리가 복제 작업을 위해서. 또는 mySQL에 전달된 데이터를 동시에 Oracle에 전달하고자 할 때에도 `Kafka Connect`을 이용해서 작업을 할 수 있다.

![image](https://user-images.githubusercontent.com/31242766/198813585-523cca29-5fe4-45b3-9a08-68c4241d20e0.png)
