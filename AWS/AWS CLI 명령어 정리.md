# AWS CLI
## key pair 조회
```
aws ec2 describe-key-pairs --query "KeyPairs[*].KeyName" --output table
```
- `--query "KeyPairs[*].KeyName"` : 모든 키페어의 이름을 가져옵니다.
- `--output table` : 결과를 테이블 형식으로 출력합니다.
