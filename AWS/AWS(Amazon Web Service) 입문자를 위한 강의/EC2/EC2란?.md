# EC2란?
## 목차
* **[EC2(Elastic Compute Cloud)](#EC2(Elastic-Compute-Cloud))**

## EC2(Elastic Compute Cloud)
### EC2 사용시 내는 다양한 지불 방법
- `On-demand` : 오랜시간도안 선불을 내지 않고 최소한의 비용을 지불하여 EC2인스턴스를 사용하고 싶을 때. 특히 앱/프로그램 개발시 최초로 EC2인스턴스에 deploy할 때 매우 유용하다.
- `Reserved` : 안정되고 예상 가능한 workload시 Reserved 사용 권장, 선불로 인한 컴퓨팅 비용 대폭 감소
- `Spot` : 단순히 비용 절감 시 유용하다. 인스턴스의 시작/끝시점에 구애받지 않을 경우 권장한다.

__EC2를 사용하기 위해 EBS라는 디스크 볼륨을 요구한다.__
