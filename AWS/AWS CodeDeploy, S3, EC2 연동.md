# AWS CodeDeploy, S3, EC2 연동
## [Github] Secrets 설정
`[Github] Secrets 설정` 은 나중에 `Github Action` 을 사용하여 `CD` 를 설정하기 위한 작업이다.    

참고 : https://github.com/haeyonghahn/TIL/blob/master/GithubAction/CI%2CCD%20%EC%84%A4%EC%A0%95.md

우선 workflow.yaml에서 사용 할 수 있도록, aws에 접근 가능한 AWS-ACCESS-KEY와 SECRET-KEY를 Github에 설정해야 합니다. 참고로 이화면 생성하는 변수들은 전부 workflow.yaml에서 사용가능하며, slack webhook url 이나 암호가 필요한 여러 변수들을 입력하여 사용 할 수 있습니다.

![image](https://user-images.githubusercontent.com/31242766/186608109-40c52dd8-5f38-4a7b-a454-ca934b5fc914.png)

ACCESS_KEY 와 SECRET_KEY 키는 IAM의 액세스 키(액세스 키 ID 및 비밀 액세스 키)를 넣어줍니다.

![image](https://user-images.githubusercontent.com/31242766/186609166-71ed3f4b-4224-4ada-a1c6-af72840d6943.png)

## [AWS] S3 Bucket 생성
배포될 소스가 저장되고 AWS-Codedeploy에서 사용할 S3 Bucket을 생성 합니다. 해당 설정 이외에는 비활성화를 선택하고 버킷을 생성합니다.

![image](https://user-images.githubusercontent.com/31242766/186610479-58c5f1aa-0873-4da5-b80b-31ea80f93465.png)
![image](https://user-images.githubusercontent.com/31242766/186612238-2fe3c970-5670-47c4-9694-2e234983c72c.png)
![image](https://user-images.githubusercontent.com/31242766/186609685-bed2fd5d-8e8f-4b8a-a0c6-a86e10053790.png)

## [AWS] ROLE 생성
배포되어야 할 EC2 instance에서 사용할 role과 codedeploy에서 사용할 role을 각각 생성 해줍니다.

### EC2 intance role
![image](https://user-images.githubusercontent.com/31242766/186613048-0c7fcdcf-81d2-423c-bde4-d94d75b94ddf.png)
![image](https://user-images.githubusercontent.com/31242766/186613418-4cb4f461-65dd-4212-88a8-77347cf5cad4.png)
![image](https://user-images.githubusercontent.com/31242766/186614053-b1a9bba6-1bbf-4b9f-a269-5dbc8d2fc923.png)
![image](https://user-images.githubusercontent.com/31242766/186614391-c0f6ea4e-f706-4273-8595-f47222940a46.png)

### codedeploy role
![image](https://user-images.githubusercontent.com/31242766/186614866-2765005e-60e5-454e-82a2-5a8176947caa.png)
![image](https://user-images.githubusercontent.com/31242766/186615000-e20d1ee9-ccd0-4b9f-b294-bbc7b37a06d9.png)
![image](https://user-images.githubusercontent.com/31242766/186615230-1788c4c3-2245-4f04-91d4-1d359ae4ad42.png)

### EC2 intance role 와 codedeploy role 생성 완료
![image](https://user-images.githubusercontent.com/31242766/186615536-34d21abd-11e8-4bc9-bded-ab0b58f0aad8.png)

## [AWS] EC2 instance 생성 및 role 설정
![image](https://user-images.githubusercontent.com/31242766/186616281-307df872-fdc4-4816-ad0f-b63dc7eb72cc.png)
![image](https://user-images.githubusercontent.com/31242766/186616421-c67a64f3-321a-43ab-88ba-04efcdde6271.png)

## [AWS] codedeploy 설정
![image](https://user-images.githubusercontent.com/31242766/186616803-159c37d8-b9d1-4c54-a0c2-c73b7b246623.png)
![image](https://user-images.githubusercontent.com/31242766/186617117-88b45d9d-788d-47aa-9d2c-6bf889224b16.png)

생성된 애플리케이션에 배포 그룹을 생성합니다.

![image](https://user-images.githubusercontent.com/31242766/186617683-7df90dc1-16e2-4326-b932-3ce8af1b9dce.png)
![image](https://user-images.githubusercontent.com/31242766/186617969-23c3c00c-8a36-4cf3-911f-bb82b708c92a.png)
![image](https://user-images.githubusercontent.com/31242766/186618287-e941ae4b-7229-4e26-9333-450c6e0d105b.png)
![image](https://user-images.githubusercontent.com/31242766/186618396-d0fd1fa6-c228-482d-a591-42312ccc56c2.png)

## [AWS] codedeploy agent 설치
이제 codedeploy가 소스를 배포할 ec2-인스턴스에 codedeploy agent를 설치합니다.
```linux
sudo yum install ruby
```
#### 이전 캐싱정보가 있으면 아래 스크립트를 작성하여 실행합니다.
```linux
#!/bin/bash
CODEDEPLOY_BIN="/opt/codedeploy-agent/bin/codeploy-agent"
$CODEDEPLOY_BIN stop
yum erase codedeploy-agent -y
```
#### 헷갈리면 `예시)` 대로 `wget` 실행한다. 실행하면 `install` 파일이 하나 생성된다.
```linux
cd /home/ec2-user
wget https://bucket-name.s3.region-identfier.amazonaws.com/latest/install
예시) wget aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install
```
#### 권한
```linx
chmod +x ./install
```
#### 최신버전 codedeploy 설치
```linux
sudo ./install auto
```
#### 특정버젼 codedeploy 설치
```linux
sudo ./install auto -v releases/codedeploy-agent-###.rpm
```
#### 서비스 실행 확인
```linux
sudo service codedeploy-agent status
## 실행중 : The AWS CodeDeploy agent is running as PID 7743
```
#### 서비스 실행
```linux
sudo service codedeploy-agent start
```
