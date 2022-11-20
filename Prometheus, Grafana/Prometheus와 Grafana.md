# Prometheus
## Prometheus 설치  
https://prometheus.io/download/

![image](https://user-images.githubusercontent.com/31242766/202903708-328beda3-e52c-459e-b312-9236607cedb3.png)

## prometheus.yml 파일 수정
- target 지정     
어디에서 정보를 지정해올 것인지  `target`을 설정한다.

## Prometheus 서버 실행
![image](https://user-images.githubusercontent.com/31242766/202903914-4b9990a0-a6e1-44a1-a53f-e7f5b0b47635.png)
```cmd
./prometheus --config.file=prometheus.yml
```
![image](https://user-images.githubusercontent.com/31242766/202903977-46eb1221-611d-4c4a-aa56-3ac851fee756.png)

## Prometheus Dashboard
- http://127.0.0.1:9000

![image](https://user-images.githubusercontent.com/31242766/202904014-aab36176-06dd-4333-bfe8-61a27f832ae5.png)

- metrics 검사 - Table

![image](https://user-images.githubusercontent.com/31242766/202904038-1c398a0b-7b26-4d8b-8183-2a5d291f0ead.png)

- metrics 검사 - Graph

![image](https://user-images.githubusercontent.com/31242766/202904063-7b11322d-c372-4994-8517-8fe9346dc116.png)

# Grafana
## Grafana 설치
