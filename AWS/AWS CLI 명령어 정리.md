# AWS CLI
## key pair 조회
```
aws ec2 describe-key-pairs --query "KeyPairs[*].KeyName" --output table
```
- `--query "KeyPairs[*].KeyName"` : 모든 키페어의 이름을 가져옵니다.
- `--output table` : 결과를 테이블 형식으로 출력합니다.

## IAM 사용자 권한 확인
특정 IAM 사용자의 권한을 확인하려면 `aws iam list-attached-user-policies` 명령어를 사용합니다.
```
aws iam list-attached-user-policies --user-name <사용자이름>
```

## IAM 그룹 권한 확인
특정 IAM 그룹에 연결된 정책을 확인하려면 `aws iam list-attached-group-policies` 명령어를 사용합니다.
```
aws iam list-attached-group-policies --group-name <그룹이름>
```

## IAM 역할 권한 확인
특정 IAM 역할에 연결된 정책을 확인하려면 `aws iam list-attached-role-policies` 명령어를 사용합니다.
```
aws iam list-attached-role-policies --role-name <역할이름>
```

## 사용자에게 직접 연결된 정책 확인
특정 사용자가 가지고 있는 인라인 정책을 확인하려면 `list-user-policies` 명령어를 사용할 수 있습니다.
```
aws iam list-user-policies --user-name <사용자이름>
```
