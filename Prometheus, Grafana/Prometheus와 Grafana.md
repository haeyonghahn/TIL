# Prometheus
## Prometheus 설치  
https://prometheus.io/download/

![image](https://user-images.githubusercontent.com/31242766/202903708-328beda3-e52c-459e-b312-9236607cedb3.png)

## prometheus.yml 파일 수정
- target 지정     
어디에서 정보를 지정해올 것인지  `target`을 설정한다.
```yml
...
# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ["localhost:9090"]
  - job_name: 'user-service'
    scrape_interval: 15s
    metrics_path: '/user-service/actuator/prometheus'
    static_configs:
    - targets: ['localhost:8000']
  - job_name: 'order-service'
    scrape_interval: 15s
    metrics_path: '/order-service/actuator/prometheus'
    static_configs:
    - targets: ['localhost:8000']
  - job_name: 'apigateway-service'
    scrape_interval: 15s
    metrics_path: '/actuator/prometheus'
    static_configs:
    - targets: ['localhost:8000']
      ...
```

## Prometheus 서버 실행
![image](https://user-images.githubusercontent.com/31242766/202903914-4b9990a0-a6e1-44a1-a53f-e7f5b0b47635.png)
- MacOS
```cmd
./prometheus --config.file=prometheus.yml
```
- Windows
```cmd
.\prometheus.exe
```

![image](https://user-images.githubusercontent.com/31242766/202903977-46eb1221-611d-4c4a-aa56-3ac851fee756.png)

## Prometheus Dashboard
- http://127.0.0.1:9090

![image](https://user-images.githubusercontent.com/31242766/202904014-aab36176-06dd-4333-bfe8-61a27f832ae5.png)

- metrics 검사 - Table

![image](https://user-images.githubusercontent.com/31242766/202904038-1c398a0b-7b26-4d8b-8183-2a5d291f0ead.png)

- metrics 검사 - Graph

![image](https://user-images.githubusercontent.com/31242766/202904063-7b11322d-c372-4994-8517-8fe9346dc116.png)

# Grafana
## Grafana 설치
https://grafana.com/grafana/download

![grafana다운로드](https://user-images.githubusercontent.com/31242766/202907219-b386c3d0-7c94-4e88-a298-b324ebbbdcf5.png)

## Grafana 실행
![image](https://user-images.githubusercontent.com/31242766/202907387-de293e59-16da-4fdc-b6ab-7731d0b489cc.png)
- MacOS
```cmd
./bin/grafana-server
```
- Windows
```cmd
.\bin\grafana-server.exe
```
![image](https://user-images.githubusercontent.com/31242766/202907526-9b230115-27d7-4528-b37a-42c59d764312.png)

## Grafana Dashboard
- http://127.0.0.1:3000
- admin / admin

![image](https://user-images.githubusercontent.com/31242766/202907653-50073703-793b-40ed-bbc4-79483e0dd26c.png)

# Prometheus-Grafana 연동
## Grafana Dashboard
https://grafana.com/grafana/dashboards

### JVM(Micrometer) 추가    
https://grafana.com/grafana/dashboards/11892-jvm/
![tempsnip](https://user-images.githubusercontent.com/31242766/202908358-79da543e-bbb5-4d46-a03d-5858aedd6141.png)
![tempsnip](https://user-images.githubusercontent.com/31242766/202908636-5c52cec0-8f6f-4243-96e2-ae683e4d6deb.png)
![image](https://user-images.githubusercontent.com/31242766/202908450-039b7539-d778-47f5-b8fc-3ff23bf0949e.png)
![image](https://user-images.githubusercontent.com/31242766/202908671-8dc5c87a-491c-41de-ada4-66cbe77639ef.png)

### Prometheus 추가
![image](https://user-images.githubusercontent.com/31242766/202907920-098e94d9-255b-45af-bac9-0a26a5869d49.png)

#### Prometheus Dashboard     
https://grafana.com/grafana/dashboards/3662-prometheus-2-0-overview/

#### Spring Cloud Gateway 추가
https://grafana.com/grafana/dashboards/11506-spring-cloud-gateway/
