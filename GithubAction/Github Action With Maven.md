## Github Action With Maven
`Spring Boot` 와 `Maven` 을 이용하여 Github Action `workflow` 작성한 `Github Action CI/CD` 를 정리한다.

```yml
# workflow의 이름
name: Spring Boot & Maven CI/CD

# workflow를 동작하게하는 trigger입니다.
# repository에 push 이벤트가 발생할 때마다 실행된다.
on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

# job은 사용자가 정한 플랫폼을 통해 step이라는 일련의 과정을 실행할 수 있다.
# 여러 개의 job을 사용할 수 있으며, 여러 개의 job을 사용할 때는 서로 정보도 교환할 수 있다.
# 그리고 각각 독립적으로도 실행할 수도 있다.
jobs:
  build:
    # 실행 환경 지정
    # 해당 job을 리눅스 환경에서 사용한다. 다른 플랫폼이 올 수도 있다.
    runs-on: ubuntu-latest

    # job 안에는 step이라는 키워드가 온다. step은 shell script를 실행할 수도 있고,
    # 누군가 만들어 놓은 Action을 사용할 수도 있다.
    steps:
      # GitHub Actions는 해당 프로젝트를 리눅스 환경에 checkout하고 나서 실행을 한다.
      # 마치 우리가 브랜치를 만들 때 checkout하는 것처럼.
      # 아래 코드는 누군가 만들어놓은 Action을 사용하는 것이다.
      # 만들어놓은 Action을 사용할 때는 uses라는 키워드를 사용해야 한다.
    - name: Step 1 - Checkout main branch from Github
      uses: actions/checkout@v3
    
    - name: Step 2 - Set up JDK 1.8
      uses: actions/setup-java@v1
      # with라는 키워드로 Action에 값을 전달할 수 있다.
      # 해당 Action은 java-version이라는 값을 받을 수 있다.
      with:
        java-version: 1.8
      # Step 3 부터 Step 12 까지 한 단계씩 shell script를 실행한다.
    - name: Step 3 - Build with Maven
      run: mvn -B package --file pom.xml
      
    - name: Step 4 - List the current directory
      run: ls -a
      
    - name: Step 5 - What is in the target folder
      run: |
        cd target
        ls -a
        
    - name: Step 6 - Make directory for deploy
      run: mkdir deploy
      
    - name: Stein 7 - Copy jar
      run: cp ./target/*.jar ./deploy
      
    - name: Step 8 - What is in the deploy folder
      run: |
        cd deploy
        ls -a
    
    - name: Step 9 - Copy appspec
      run: cp appspec.yml ./deploy
    
    - name: Step 10 - Copy Shell
      run: | 
        cp ./scripts/* ./deploy
        cd deploy
        ls -la
      
    - name: Step 11 - Grant execute permission
      run: chmod +x ./deploy/deploy.sh

    - name: Step 12 - Make zip file
      run: zip -r githubAction.zip ./deploy
      
      # Step 안에 있는 env 환경변수는 Step 을 위한 환경변수이다.
    - name: Step 13 - Deliver to AWS S3
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.ACCESS_KEY }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.SECRET_KEY }}
      run: |
        aws s3 cp \
        --region ap-northeast-2 \
        --acl private \
        ./githubAction.zip s3://my-2andgo-bucket/
    - name: Step 14 - Deploy with AWS codeDeploy
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.ACCESS_KEY }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.SECRET_KEY }}
      run: |
        aws deploy create-deployment \
        --application-name gitubaction \
        --deployment-group-name githubaction-group \
        --file-exists-behavior OVERWRITE \
        --s3-location bucket=my-2andgo-bucket,bundleType=zip,key=githubAction.zip \
        --region ap-northeast-2
```
## 참고
https://callmemaru.com/posts/github-actions-learning-3/       
https://fe-developers.kakaoent.com/2022/220106-github-actions/
