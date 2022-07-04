# AWS 요금
## Elastic IP
Elastic IP 주소는 ip주소를 고정으로 사용할 수 있도록 해주는 서비스이다. EC2가 stop/start 되는 경우 ip주소가 매번 변경되는데 이를 EC2에 연결해두고 Elastic ip주소로 접근하면 항상 같은 주소로 접근할 수 있게 된다.

- 프리티어에서 Elastic IP 1개를 무료로 사용할 수 있다.
- 하지만 Elastic IP는 EC2에 연결해두지 않으면 요금이 청구된다.
- IP가 부족한 상황에서 Elastic IP를 만들어두고 EC2에 연결하지 않으면 IP가 만들어져 있지만 사용되지 않고 있으므로 요금이 청구된다.
- 또한 EC2에 연결해두었더라도 EC2가 stop되어있는 상태라면 요금이 청구된다.
- 만약 Elastic IP를 만들어두고 할당을 하지 않은 상태라면 실행중인 EC2에 할당 혹은 Elastic ip를 삭제해야한다.

## RDS
MySQL, PostgreSQL, MariaDB, Oracle BYOL 또는 SQL Server를 위한 관리형 관계형 데이터베이스 서비스이다.

- RDS 인스턴스 1개 무료 사용 가능
- 월별 750시간까지 무료
- 단, 해당 DB엔진이 db.t2.micro 타입만 사용 가능
- 범용(SSD) 데이터베이스 스토리지 20GB 제한. 만일 10GB를 사용하는 RDS 인스턴스 3개를 생성하면 과금이 되게 된다 (30GB이 되니까)
- 데이터베이스 백업 및 DB 스냅샷용 스토리지 20GB
- RDS 생성할때 오토 백업 안되게 주의 
- RDS 스토리지 자동 조정 옵션 끄기 
- Multi-AZ와 고성능 I/O인 Provisioned IOPS Storate를 사용하지 않도록 설정

## ElastiCache
프리티어에서 ElastiCache 1개는 무료로 사용할 수 있다. 무료 사용 대상은 `t2.micro`이다.

## EBS(Elastic Block Store)
### EBS란 ?
EBS란 Elastic Block Store의 약자로 일종의 하드디스크라고 생각하면 된다. EBS의 특징은 아래와 같다.

- 필요한 용량에 맞게 구입 할 수 있다. 이를테면 EC2 인스턴스를 웹서버의 용도로만 쓰고 파일 저장은 S3를 사용한다면 넉넉 잡고 1기가바이트만 구입하면 된다.
- 필요에 따라서 즉시 생성하고, 제거 할 수 있다.
- 사용한 만큼 과금 되는 종량제다. 자세한 내용은 설명서의 비용예측을 참고한다.
- 내부적으로 데이터를 실시간 복제하고 있기 때문에 하드디스크에 비해서 데이터를 잃어버릴 확률이 현저히 낮다.
- 스냅샷 기능을 제공해서 EBS의 현재 상태 그대로 보존 할 수 있다.
- CloudWatch를 통해서 EBS의 통계를 열람 할 수 있다.
- EC2 인스턴스를 제거해도 EBS는 독립적이기 때문에 데이터가 유지 된다.

### Volume 이란 ?
EBS로 생성한 (말하자면) 디스크 하나 하나를 볼륨이라고 부른다.

![EBS](https://github.com/haeyonghahn/TIL/blob/master/AWS/images/EBS.png)

- Size : EBS의 용량
- Availability Zone : 하나의 지역(region)에는 물리적으로 분리된 복수의 가용성 존이 있다. 물리적으로 분리되었다는 의미는 서로 다른 지역에 데이터 센터가 위치하고 있다는 의미다. 그러면서도 각각의 데이터 센터는 네트워크 상으로는 가까운 거리를 유지하고 있기 때문에 같은 지역(region) 내의 가용성 존에서는 데이터를 고속으로 전송할 수 있다. Global Infrastructure를 통해서 가용성 존의 상태를 열람 할 수 있다. 
- Snapshot : EBS는 현재 저장된 데이터 상태 그대로 보관할 수 있는데, 이를 스냅샷이라고 한다. 미리 만들어진 스냅샷을 선택해서 똑같은 데이터가 저장된 상태로 EBS를 새로 만들 수 있다.
- Volume Type : IOPS는 디스크에 데이터를 읽고 쓰는 속도를 의미한다. IOPS를 선택하면 EBS의 읽기/쓰기 속도를 선택할 수 있고, 표준(standard)를 선택하면 IOPS의 값이 100으로 기본설정된다. (통상 7500rpm 속도의 하드디스크의 IOPS를 75~100 정도로 잡는다)

### EBS 요금 발생
EC2생성 시 기본세팅을 조정하지 않았다면 EC2 1개당 8GB(최대30GB)의 EBS가 생성된다. 프리티어 사용자라면 EC2를 1개만 사용할 것이기 때문에 전혀 문제가 되지 않을 것이라고 생각할 수 있다. 하지만 문제는 `EC2를 stop하면 요금은 청구되지 않지만 EBS는 여전히 사용중인 것으로 된다.` 예를 들어,

- 오전 10시에 EC2 1개를 생성하고 나서 30분 뒤에 stop시킨다.
- 오전 11시에 EC2 1개를 생성하고 나서 40분 뒤에 stop시킨다.
- 그렇게 반복적으로 총 6시간 동안 6개의 EC2를 생성하고 1시간 안에 stop할 경우 프리티어로서 EC2 사용시간은 총 750시간에 전혀 영향을 미치지 않는다.
- 총 6개의 EC2를 생성했지만 1시간에 1개의 EC2만 사용했으므로 문제가 없는 것이다.
- 하지만 문제는 EC2를 `중지(terminate)`시키지 않고 `종료(stop)`만 시켰다는 것이다.
- EC2를 terminate시킬 경우 함께 만들어진 EBS도 없어지게 된다.

![EBS예시](https://github.com/haeyonghahn/TIL/blob/master/AWS/images/EBS%EC%98%88%EC%8B%9C.PNG)

위의 경우처럼 EC2 6개를 생성하고 STOP만 해두었다면 6개의 EBS볼륨은 그대로 남아있게 된다.   
8GB X 6개 = 48GB를 사용하고 있으므로 프리티어 30GB를 초과하게 되어 요금이 발생한다. 사용하지 않는 EC2가 있다면 `stop`이 아닌 `terminate`를 시켜주어 EBS 사용량 초과로 요금이 발생하는 것을 막아야한다.

## 출처
- https://inpa.tistory.com/entry/AWS-%F0%9F%92%B0-%ED%94%84%EB%A6%AC%ED%8B%B0%EC%96%B4-%EC%9A%94%EA%B8%88-%ED%8F%AD%ED%83%84-%EB%B0%A9%EC%A7%80-%F0%9F%92%B8-%EB%AC%B4%EB%A3%8C-%EC%82%AC%EC%9A%A9%EB%9F%89-%EC%A0%95%EB%A6%AC#AWS_%ED%94%84%EB%A6%AC%ED%8B%B0%EC%96%B4_%EA%B3%BC%EA%B8%88_(AWS_Reduce_Cost)
- https://gun0912.tistory.com/45
