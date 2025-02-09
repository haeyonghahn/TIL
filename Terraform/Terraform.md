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
키페어 등록을 위한 sshkey 생성
- C 드라이브에 `sshkey` 폴더를 생성한다.
```cli
PS C:\Users\user> ssh-keygen -t rsa -b 4096 -C "aws_keyfair" -f "C:\\sshkey/tf_keypair"
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\\sshkey/tf_keypair
Your public key has been saved in C:\\sshkey/tf_keypair.pub
The key fingerprint is:
SHA256:YjcBloLC6x9KMchs9Hz0GdrFcg20mSw/xQZgeeqGpyQ aws_keyfair
The key's randomart image is:
+---[RSA 4096]----+
|.  .  o+=+o      |
|.o. .oo=.=*.     |
|=.+ ..+.X= +     |
|.B o o =o.o      |
|o o . = So       |
| o E + * ..      |
|. o + +          |
| . . .           |
|                 |
+----[SHA256]-----+
```

### Terraform 을 이용한 AWS 리소스 생성
```cli
PS C:\terraform\01_tf> terraform init
PS C:\terraform\01_tf> terraform plan
PS C:\terraform\01_tf> terraform destroy
```

## Terraform 을 이용한 VPC 구성
