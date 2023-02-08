# EC2
해당 문서는 [AWS(Amazon Web Service) 입문자를 위한 강의](https://www.inflearn.com/course/aws-%EC%9E%85%EB%AC%B8/dashboard)를 토대로 작성되었습니다.

## 목차
* **[EC2(Elastic Compute Cloud)](#ec2elastic-compute-cloud)**
* **[EBS(Elastic Block Storage)](#ebselastic-block-storage)**
* **[ELB(Elastic Load Balancers)](#elbelastic-load-balancers)**

## EC2(Elastic Compute Cloud)
### EC2 사용시 내는 다양한 지불 방법
- `On-demand` : 오랜시간도안 선불을 내지 않고 최소한의 비용을 지불하여 EC2인스턴스를 사용하고 싶을 때. 특히 앱/프로그램 개발시 최초로 EC2인스턴스에 deploy할 때 매우 유용하다.
- `Reserved` : 안정되고 예상 가능한 workload시 Reserved 사용 권장, 선불로 인한 컴퓨팅 비용 대폭 감소
- `Spot` : 단순히 비용 절감 시 유용하다. 인스턴스의 시작/끝시점에 구애받지 않을 경우 권장한다.

__EC2를 사용하기 위해 EBS라는 디스크 볼륨을 요구한다.__

## EBS(Elastic Block Storage)
- EC2 인스턴스의 가상 하드디스크이다.
- 저장 공간이 생성되어지며 EC2 인스턴스에 부착된다.
- 디스크 볼륨 위에 File System이 생성된다.
- EBS는 특정 Availability Zone에 생성된다.

### Availability Zone (AZ)
![image](https://user-images.githubusercontent.com/31242766/215039528-1a687134-b4e8-4428-b1ec-2134f60c01d2.png)

### EBS 볼륨 타입
__SSD군__   
- General Purpose SSD (GP2) : 최대 10K IOPS를 지원하며 1GB당 3IOPS 속도가 나온다.
- Provisioned IOPS SSD (IO1) : 극도의 I/O률을 요구하는(예시: 매우 큰 DB관리) 환경에서 주로 사용됨. 10K 이상의 IOPS를 지원한다.    

__Magnetic/HDD군__   
- Throughput Optimized HDD (ST1) : 빅데이터 Datawarehouse, Log프로세싱시 주로 사용한다. (boot volume으로 사용 가능 X)
- CDD HDD (SC1) : 파일 서버와 같이 드문 volume 접근시 주로 사용, 역시 boot volume으로 사용 불가능하나 비용은 매우 저렴하다.
- Magnetic (Sandard) : 디스크 1GB당 가장 싼 비용을 자랑함. Boot volume으로 유일하게 가능하다.

## ELB(Elastic Load Balancers)
- 수많은 서버의 흐름을 균형있게 흘려보내는데 중추적인 역할을 한다.
- 하나의 서버로 traffic이 몰리는 병목현상(bottleneck) 방지할 수 있다.
- Traffic의 흐름을 Unhealthy instance -> healthy instance로 흘러가도록 할 수 있다.

### Load Balancer 종류
1. `Application Load Balancer` : OSI Layer7에서 작동된다.
- HTTP, HTTPS와 같은 traffic의 load balancing에 가장 적합하다.
- 고급 request 라우팅 설정을 통하여 특정 서버로 request를 보낼 수 있다.

2. `Network Load Balancer` : OSI Layer4에서 작동됨, 매우 빠른 속도를 자랑하며 Production환경에서 종종 쓰인다.
- 극도의 performance가 요구되는 TCP traffic에서 적합하다.
  - 초당 수백만개의 request를 아주 미세한 delay로 처리 가능

3. `Classic Load Balancer` : 현재 Legacy로 간주됨, 따라서 거의 쓰이지 않는다.
- Layer7의 HTTP/HTTPS 라우팅 기능 지원
- Layer4의 TCP traffic 라우팅 기능도 지원

### Load Balancer Error : 504 ERROR
![image](https://user-images.githubusercontent.com/31242766/215044636-30072f74-c162-4ae3-863e-c8395084889b.png)

### X-Forwarded-For 헤더
`X-Forwarded-For` (XFF) 헤더는 HTTP 프록시나 로드 밸런서를 통해 웹 서버에 접속하는 클라이언트의 원 IP 주소를 식별하는 사실상의 표준 헤더다. 클라이언트와 서버 중간에서 트래픽이 프록시나 로드 밸런서를 거치면, 서버 접근 로그에는 프록시나 로드 밸런서의 IP 주소만을 담고 있다. 클라이언트의 원 IP 주소를 보기위해 X-Forwarded-For 요청 헤더가 사용된다.

__문법__   
```console
X-Forwarded-For: <client>, <proxy1>, <proxy2>
```

__지시자__    
`<client>`   
클라이언트 IP 주소   
`<proxy1>, <proxy2>`   
하나의 요청이 여러 프록시들을 거치면, 각 프록시의 IP 주소들이 차례로 열거된다. 즉, 가장 오른쪽 IP 주소는 가장 마지막에 거친 프록시의 IP 주소이고, 가장 왼쪽의 IP 주소는 최초 클라이언트의 IP 주소다.   

__예제__   
```console
X-Forwarded-For: 2001:db8:85a3:8d3:1319:8a2e:370:7348

X-Forwarded-For: 203.0.113.195

X-Forwarded-For: 203.0.113.195, 70.41.3.18, 150.172.238.178
```

![image](https://user-images.githubusercontent.com/31242766/215045422-f1fd2ed3-7ea7-41e0-91cb-50838a3dcb67.png)
