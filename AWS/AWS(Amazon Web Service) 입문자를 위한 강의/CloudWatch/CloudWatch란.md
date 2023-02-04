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
EC2 인스턴스를 생성하여 메모리 가능 사용 여부를 확인하는 CloudWatch를 만들어보자.

__EC2 인스턴스 생성__   
![image](https://user-images.githubusercontent.com/31242766/216757570-fbf56a40-f899-4821-9ce2-569554813d92.png)

__datagrip 파일 다운로드__     
[링크](https://download.jetbrains.com/datagrip/datagrip-2020.1.5.dmg?_ga=2.262501066.737529424.1673857455-953513752.1673857455&_gl=1*1ml80rw*_ga*OTUzNTEzNzUyLjE2NzM4NTc0NTU.*_ga_9J976DJZ68*MTY3Mzg1NzQ1NC4xLjAuMTY3Mzg1NzQ1OS4wLjAuMA..)    
해당 파일을 EC2 인스턴스에 전송함으로써 cloudWatch 임계값을 설정하여 Alarm을 울려보자.

__EC2 인스턴스에 datagrip 파일 전송__   
```linux
scp -i awslearner-cw.pem datagrip-2020.1.5.dmg ec2-user@ec2-54-180-125-70.ap-northeast-2.compute.amazonaws.com:datagrip.dmg
```
- `scp` : secure copy protocol 의 약자로서 서버 A에서 서버 B로 전송할 때 사용하는 프로토콜이다.

![image](https://user-images.githubusercontent.com/31242766/216758306-b051bc2f-516a-4cb1-8532-29bb1f82d53e.png)
![image](https://user-images.githubusercontent.com/31242766/216758337-4d1e42f4-0d81-473b-88c0-eeb4e8b8e22f.png)

__CloudWatch 대시보드__    
![image](https://user-images.githubusercontent.com/31242766/216758402-1089f83e-1fa0-4dee-8398-2ad878640f19.png)
![image](https://user-images.githubusercontent.com/31242766/216758428-68f11f17-f910-453b-a2d0-c123ff470341.png)

EC2 인스턴스 모니터링에서도 확인할 수 있다.   
![image](https://user-images.githubusercontent.com/31242766/216758469-47ad2e3d-0c64-4ad9-9371-c206dae6ccfe.png)

세부 모니터링을 사용하여 주기를 짧게 할 수 있다.(유료)   
![image](https://user-images.githubusercontent.com/31242766/216758502-5c56c717-06c8-48c9-aad4-c5a24f9d11fe.png)

__경보__   
![image](https://user-images.githubusercontent.com/31242766/216758543-05519f8f-aa79-46de-923f-8fde62b2b1bb.png)   
EBS의 `VolumeWriteBytes`로 테스트해보자.   
![image](https://user-images.githubusercontent.com/31242766/216758612-6d1b3f95-ffda-42e1-8af6-444bea3d1011.png)    
`VolumeId`는 EC2 인스턴스 생성시 생성되었던 `EBS Vloume` ID 이다.
![image](https://user-images.githubusercontent.com/31242766/216758772-b52758e8-a52e-4ca4-b634-51114a6e30ba.png)   
알람을 설정했음에도 불구하고 데이터가 들어오지 않을 때 데이터를 어떻게 처리하는지에 대한 행동을 설정하는 것이다.    
![image](https://user-images.githubusercontent.com/31242766/216758787-db0d178d-12f7-4f7f-9518-823869901c0f.png)    
알람을 어떻게 보낼지 설정한다.   
![image](https://user-images.githubusercontent.com/31242766/216758934-1cb94c1c-bd8f-417b-b6fc-f1458f10cf77.png)    
- Auto Scaling 작업 : 예를 들어, 메모리 부족 현상이 발생했을 때 경보가 발생하며 메모리를 더 추가할 수 있도록 한다. 
- EC2 작업 : 현재 EBS Metrics를 사용할 것이기 때문에 비활성화되어 있다.
- Systems Manager 작업 :  경보가 발생했을 때 경보의 심각성을 수치화하여 얼마나 신속히 대처할 수 있는지 인사이트를 보기 원할 때 사용되어진다.   

![image](https://user-images.githubusercontent.com/31242766/216759110-269dfc39-9c28-441d-b81d-31247c74ff5f.png)

![image](https://user-images.githubusercontent.com/31242766/216759182-e96ab843-0cc1-4cea-bfcb-8fe1eb1ca678.png)
![image](https://user-images.githubusercontent.com/31242766/216759223-a462e940-e1d9-4d72-a114-f54a92db651c.png)

__데이터를 다시 전송__    
datagrip.dmg를 다시 전송하여 CloudWatch를 확인해보자.
```linux
scp -i awslearner-cw.pem datagrip-2020.1.5.dmg ec2-user@ec2-54-180-125-70.ap-northeast-2.compute.amazonaws.com:datagrip2.dmg
```
![image](https://user-images.githubusercontent.com/31242766/216759505-e8d6b9f2-9159-4623-8180-1612b8debd0f.png)
![image](https://user-images.githubusercontent.com/31242766/216759539-ac759ec5-8148-43f4-8091-e913eacd670e.png)
![image](https://user-images.githubusercontent.com/31242766/216759576-e9623bd9-fb74-47e7-9cae-12eb23cdbc6a.png)
