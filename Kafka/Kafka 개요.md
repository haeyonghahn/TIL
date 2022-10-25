# Kafka
## Kafka란?
- 실시간 데이터 피드를 관리하기 위해 통일된 높은 처리량, 낮은 지연 시간을 지닌 플랫폼이다.
- 모든 시스템으로 데이터를 실시간으로 전송하여 처리할 수 있는 시스템이다.
- 데이터가 많아지더라도 확장이 용이한 시스템이다.

중간에 `kafka`를 도입하게되면서 mySQL, Oracle, mongoDB, 어플리케이션, 데이터 스토리지 등 자신들이 전송하는 데이터가 어떠한 시스템에 저장되는지에 관계없이 하둡이라든지, 모니터링 시스템 등에 전달해줄 때  `kafka` 쌓여 있는 메시지들이 단일 포맷으로 전달될 수 있도록 한다. 어떤 곳에서 보내든, 누가 받든 신경쓰지 않고 메시지를 주고 받는 것이 가능해진다.

- Producer/Consumer 분리
- 메시지를 여러 Consumer에게 허용
- 높은 처리량을 위한 메시지 최적화
- scale-out 가능

![image](https://user-images.githubusercontent.com/31242766/197794620-f1c47792-62c9-4a0c-9557-2a20738ce481.png) 

## Kafka Broker
- 실행된 Kafka 애플리케이션 서버
- 3대 이상의 Broker Cluster 구성
- Zookeeper 연동
  - 메타데이터(Broker ID, Controller ID 등) 저장 
  - Controller 정보 저장
- n개 Broker 중 1대는 Controller 기능 수행
  - Controller 역할
    - 각 Broker에게 담당 파티션 할당 수행
    - Broker 정상 동작 모니터링 관리

![image](https://user-images.githubusercontent.com/31242766/197796709-4d662617-feb2-4271-baef-43e1b27eb776.png)

- `KafkaCluster` : 카프카의 브로커들의 모임이다. Kafka는 확장성과 고가용성을 위하여 broker들이 클러스터로 구성되어 있다.
- `Broker` : 각각의 카프카 서버이다. 동일 노드에 여러 브로커를 띄울 수 있다.
- `Zookeeper` : 카프카 클러스터 정보 및 분산처리 관리 등 메타데이터를 저장한다.
- `Producer` : 메시지(이벤트)를 발행하여 생산(Write)하는 주체
- `Consumer` : 메시지(이벤트)를 구독하여 소비(Read)하는 주체

## 참고
https://ifuwanna.tistory.com/487    
