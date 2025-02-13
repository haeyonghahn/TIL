### Terraform 설치
https://developer.hashicorp.com/terraform/install?product_intent=terraform

### AWS CLI 설치
https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html

### AWS CLI 를 이용한 AWS 계정 연동
프롬프트에서 AWS IAM 계정 연동을 실시한다.
```cli
PS C:\User\user> aws configure
AWS Access Key ID [None] : 
AWS Secret Access Key [None] :
Default region name [None] : ap-northeast-2
Default output format [None] : json
```
디렉토리에 `.aws` 폴더에 인증 파일(`config`, `credentials`)이 생성되었는지 확인한다.
```cli
PS C:\Users\user> aws configure list
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************S6N7 shared-credentials-file
secret_key     ****************bLap shared-credentials-file
    region           ap-northeast-2      config-file    ~/.aws/config
```

### Terraform 실행 체크리스트
| 단계 | 명령어 | 설명 |
|------|-------------------------------|----------------------------------|
| ✅ 문법 및 구성 검증 | `terraform validate` | 문법 오류 및 변수 정의 확인 |
| ✅ 변경 사항 미리 보기 | `terraform plan` | 실제로 적용될 변경 사항 확인 |
| ✅ 코드 정리 | `terraform fmt -recursive` | 코드 스타일 자동 정리 |
| ✅ Best Practice 체크 | `tflint` | 코드 분석 및 보안 문제 탐지 |
| ✅ 환경 변수 검증 | `terraform console` | 변수 값 미리 확인 |
| ✅ AWS 연결 테스트 | `aws sts get-caller-identity` | AWS 권한 및 연결 상태 확인 |

