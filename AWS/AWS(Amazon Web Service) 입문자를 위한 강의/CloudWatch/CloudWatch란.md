# CloudWatch
## 목차
* **[CloudWatch](#CloudWatch)**
* **[Alarm](#alarm)**
* **[실습](#실습)**

## CloudWatch
- AWS 리소스 사용의 실시간 모니터링 기능 지원
- 다양한 이벤트들을 수집하여 로그파일로 저장
- 이벤트&알람 설정을 통해 SNS, AWS Lambda로 전송 가능
- [CloudWatch 사용 가능 서비스들] : EC2, RDS, S3, ELB, 등 등
  - S3버켓 파일 업로드 & 삭제
  - S3버켓 접근 시 접근 거부 발생
  - RDS 데이터베이스에 접속 시도

### CloudWatch 모니터링 종류
1. Basic Monitoring
- 무료
- 5분 간격으로 최소의 Metrics 제공
2. Detailed Monitoring
- 유료
- 1분 간격으로 자세한 Metrics 제공

### CloudWatch 사용 사례 - 1
- Use Case : 매일 얼마나 많은 사용자들이 모바일 앱을 사용하는지 알고 싶다.
- Potential Issue : 특정날에 수많은 `traffic`이 몰릴 수 있어 병목 현상이 생길 수 있다.
- Solution : 매일 `traffic rate`와 특정 버튼의 유저 클릭 횟수를 분석하여 더 효율적인 앱 개발을 할 수 있는 통찰력을 얻을 수 있다.

### CloudWatch 사용 사례 - 2
- Use Case : 특정 시간대에 웹서버 상태를 점검하여 비용 절감 목표
- Potential Issue : 똑같은 비용을 내며 AWS 리소스들을 사용하지만 낮시간대와 밤시간대에 필요한 서버의 성능은 달라질 수 있기 때문에 금전적 손실이 생길 수 있다.
- Solution : 알람 설정을 통하여 특정 `threshold`에 도달했을 때 개발자에게 상황을 보고해줌으로서 서버 관리를 할 수 있다.

## Alarm
- 임의로 정해놓은 값에 도달할 시 Alarm을 울린다.
- Alarm이 울릴 시 특정 이벤트들을 작동시킬 수 있다.

### Alarm State
- Alarm : 어떤 Metrics가 임의로 정해놓은 `threshold` 값을 벗어났을 때 생기는 상황이다. 즉, 알람이 울린 상태이다.
- Insufficient : 해당 상태는 예를 들어, EC2 인스턴스의 메모리 누수 알람을 만들었다고 하자. 하지만 EC2 인스턴스를 만들지 않고 오랜 시간동안 해당 상태가 지속이 된다면 해당 알람은 `Insufficient` 상태이다. 왜냐하면 EC2 인스턴스가 존재하지 않기 때문에 이에 따른 Metrics가 생성되지 않는다.
- OK : 알람이 울리지 않고 원하는 범위 내에서 리소스가 돌아간다는 의미이다.

## 실습
