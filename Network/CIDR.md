# CIDR (Classless Inter-Domain Routing)
네트워크 설계를 하면서 가장 많이 접하게 될 개념이 CIDR(사이더) 이다.
CIDR의 full name은 (Classless Inter-Domain Routing) 으로 `클래스 없는 도메인간 라우팅 기법`이라는 뜻을 내포한다.
즉, 도메인간의 라우팅에 사용되는 인터넷 주소를 원래 IP주소 클래스 체계를 쓰는 것보다 더욱 능동적의로 할수 잇도록 할당하여 지정하는 방식중 하나 이다.

언뜻 보면, 우리가 IP주소 클래스를 배우면서 같이 배우는게 서브넷 마스크 그리고 서브네팅인데, `서브네팅과 차이`가 무엇인지 애매할 때가 있다.
서브네팅 자체가 IP클래스에 국한되지 않고 더욱더 IP 주소를 쪼개는 방식을 말하는건데 이게 바로 클래스 없는 도메인간 라우팅 기법이기 때문이다.
결론적으로 말하면,

`서브네팅 ⊂ CIDR`   
이런 집합 관계가 형성된다.   
서브네팅 뿐만아니라 서브넷을 합치는 슈퍼네팅 역시 CIDR의 일환이다.   
정리하자면, 서브네팅, 슈퍼네팅 이러한 IP나누고 합치는 기법을 모두 CIDR이라고 이해하면 된다.   

## CIDR 표기법 이란?
CIDR를 정의하는 설명중에, `CIDR은 네트워크 정보를 여러개로 나누어진 Sub-Network들을 모두 나타낼 수 있는 하나의 Network로 통합해서 보여주는 방법이다.` 라고 하는데
즉, 아이피와 서브넷 마스크를 다음과 같이 표기한 것을 말한다.   
`192.168.10.70/26`   
서브네팅 배울때 서브넷 마스크를 짧게 적은 prefix라고 배웠겠지만 이것이 바로 `CIDR 이자 CIDR 표기법` 이다.   
이런 식으로 표기하는 이유는 단 한줄만으로 네트워크 범위를 추측 또는 측정 할 수 있기 때문이다.

예를 들어,
누가 나한테 찾아와서 "우리 회사는 50개 아이피를 쓰는데 제 아이피 주소가 192.168.10.70 이에요. 
아이피를 다른 번호로 할당받고싶은데 아이피 범위가 어디에서 어디까지 인지 몰라서 어떤 주소 번호를 할당을 요청해야 할지 모르겠어요. 
우리 회사 아이피 할당 범위를 알고싶어요" 라고 물었다.

그럼 어떻게 대답할 것인가? `192.168.10.70` 이라함은 대역폭을 봤을때 `ip C클래스(192.0.0.0 ~ 223.255.255.0)`에 속한다.   
C클래스면 기본적인 디폴트 서브넷 마스크는 255.255.255.0 일 것이다.
>디폴트 서브넷 마스크 (Default Subnet Mask) 란?   
>IP 주소 클래스 범위에서 서브넷을 나누지 않고 사용하는 경우 적용되는 서브넷 마스크가 디폴트 서브넷 마스크라고 이해하면 된다.
> - 클래스 C 디폴트 서브넷 마스크 : 255.255.255.0
> - 클래스 B 디폴트 서브넷 마스크 : 255.255.0.0
> - 클래스 A 디폴트 서브넷 마스크 : 255.0.0.0

그러면 `네트워크 ID는 192.168.10` 일 것이며 `호스트 ID 부분은 .70` 부분일 것이다.
따라서 기본적인 아이피 범위는 `192.168.10.0 ~ 192.168.0.255` 일 것이다.
그런데 회사가 사용하는 아이피는 50개 라고 한다. 그러면 분명 서브네팅을 통해 아이피 범위를 더 한정지어서 받았을 것이다.
아이피 50를 사용하는 회사에 적절하게 분배가 가능한 범위는 C클래스의 256개의 범위를 4등분한 64개 범위이다.
64개는 2의 6승이고 이는 호스트 ID가 6개의 비트를 사용한다는 뜻이다.
그래서 64씩 나누어 범위를 표현하자면, 최종적으로 다음과 같은 대역으로 나뉘어 진다.   
![image](https://user-images.githubusercontent.com/31242766/209817475-c1552cee-7e23-4c51-a03a-9b22bdbce902.png)    
잊지 말아야 할게 각 네트워크의 첫번째와 마지막 IP는 사용이 불가능 하다. (네트워크 주소와 브로드 캐스트 주소)   

자, 그럼 물음에 대한 답변을 할 시간이다.
"당신의 아이피 192.168.10.70 로 유추하건데, 당신네 회사는 네트워크 2 (192.168.10.64 ~ 192.168.10.127) 에 속해 있네요."
라고 하면 된다.

너무 길다.   
아이피 범위 하나 알려주는데 말하고 쓸게 너무 많다.    
딱 한 줄로 표기해서 알려줄수는 없을까?   

"당신네 회사 아이피 범위요? 192.168.10.70/26입니다."   
### 192.168.10.70/26 풀이법
![image](https://user-images.githubusercontent.com/31242766/209817927-44667a3f-584d-49e1-adc9-4cfb51360cbb.png)

## CIDR 블럭 이란?
어렵게 이해할 필요없이 `CIDR블럭은 서브넷` 이다.   
AWS 진영에선 서브넷을 멋스럽게 CIDR 블럭이라고 부른다고 이해하면 된다.

예를들어, 다음과 같은 192.168.0.0/16 대역망이 있다.   
이를 세개의 네트워크 단위(서브넷)으로 쪼갠 것이다. 서브넷1, 2, 3이 바로 CIDR 블럭이다.   
![image](https://user-images.githubusercontent.com/31242766/209818198-30b5ccfb-0c21-4e5c-98a1-9030731813d9.png)

### AWS CIDR
AWS VPC를 학습할때 가장 중요하고 먼저 등장하는 개념이 바로 이 CIDR(Classless Inter-Domain Routing)이다.   
AWS 클라우드라서 다를게 없다. 공부한걸 그대로 접목시키면 된다.   
![image](https://user-images.githubusercontent.com/31242766/209818453-250a61f3-6170-4df0-a95f-35d44771c1ce.png)   
VPC를 구성할때 다음 CIDR값이 있다면, VPC는 16을 서브넷 값으로 하는 IP 주소 범위를 나타내는 것이다.   
그리고 각 자원에 줄 수 있는 IP 갯수는 2의 16승인 65,536개인 것을 간단한 CIDR 표기법으로 알려줄 수 있게 된다.   
그리고 이 IP주소는 10.0.*.* 이라는 네트워크 주소안에 포함되어 있다는 것도 한눈에 알 수 있다.   

 

다만 AWS는 일반적인 서브네팅과 `다른 차이점`이 있는데,   
보통이라면 IP주소범위의 가장 처음과 끝을 네트워크 주소(10.0.0.0) 브로드캐스트 주소(10.0.0.255)로서 `2개`만 제외하고 나머지는 사용할수있는 IP 주소겠지만, 
AWS에서는 따로 자체 클라우드에서 설정해 사용하고있는 IP가 있기 때문에 총 `5개`를 제외하여야 한다.
![image](https://user-images.githubusercontent.com/31242766/209818590-65442576-49a2-478e-871f-e6287027deef.png)   
> 참고   
> 예를들어 이런 AWS 기출 문제가 있다고하면,   
> Q. AWS에 192.168.0.0/24 에서 총 사용가능한 호스트 갯수는?   
> 1이 24개 있으니 호스트ID는 8비트니까, 2^8=256개 이고 그중 처음과 끝은 빼면 254개다! 라고 하면 틀린 것이다.   
> 256-5=251이 정답이다.

## 출처
https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-CIDR-%EC%9D%B4-%EB%AC%B4%EC%96%BC-%EB%A7%90%ED%95%98%EB%8A%94%EA%B1%B0%EC%95%BC-%E2%87%9B-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC-%EA%B3%84%EC%82%B0%EB%B2%95#192.168.10.70/26_%ED%92%80%EC%9D%B4%EB%B2%95_%E2%96%BC
