# EC2 프리티어 요금
## Elastic IP
Elastic IP 주소는 ip주소를 고정으로 사용할 수 있도록 해주는 서비스이다. EC2가 stop/start 되는 경우 ip주소가 매번 변경되는데 이를 EC2에 연결해두고 Elastic ip주소로 접근하면 항상 같은 주소로 접근할 수 있게 된다.

- 프리티어에서 Elastic IP 1개를 무료로 사용할 수 있다.
- 하지만 Elastic IP는 EC2에 연결해두지 않으면 요금이 청구된다.
- IP가 부족한 상황에서 Elastic IP를 만들어두고 EC2에 연결하지 않으면 IP가 만들어져 있지만 사용되지 않고 있으므로 요금이 청구된다.
- 또한 EC2에 연결해두었더라도 EC2가 stop되어있는 상태라면 요금이 청구된다.
- 만약 Elastic IP를 만들어두고 할당을 하지 않은 상태라면 실행중인 EC2에 할당 혹은 Elastic ip를 삭제해야한다.

## RDS
RDS도 1개는 프리티어에서 무료로 사용할 수 있다. `다만 RDS생성시 Multi-AZ와 고성능 I/O인 Provisioned IOPS Storate를 사용하지 않도록 설정해야 한다.`

기본설정으로 [Yes]가 선택되어 있기 때문에 많이 실수하는 부분이다.

## ElastiCache
프리티어에서 ElastiCache 1개는 무료로 사용할 수 있다. 무료 사용 대상은 `t2.micro`이다.

## EBS
EC2생성 시 기본세팅을 조정하지 않았다면 EC2 1개당 8GB의 EBS가 생성된다. 프리티어 사용자라면 EC2를 1개만 사용할 것이기 때문에 전혀 문제가 되지 않을 것이라고 생각할 수 있다. 하지만 문제는 `EC2를 stop하면 요금은 청구되지 않지만 EBS는 여전히 사용중인 것으로 된다.`
