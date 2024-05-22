# Linux 명령어
## 목차
* **[useradd](#useradd)**
* **[passwd](#passwd)**
* **[more](#more)**
* **[cp(Copy)](#cp(Copy))**
* **[mv(Move)](#mv(Move))**
* **[rm(Remove)](#rm(Remove))**
* **[다중 명령어](#다중-명령어)**
* **[tail](#tail)**
* **[ssh](#ssh)**
* **[su와 sudo](#su와-sudo)**

## useradd
리눅스 시스템에서 새로운 사용자를 추가할 때 사용된다.

### 예제
```linux
sudo useradd -m -g wheel -d /home/newuser -s /bin/bash -c "John Doe" newuser
```
- `-m`: /home/newuser 경로에 홈 디렉터리를 생성한다.
- `-g wheel`: 사용자를 wheel 그룹에 추가한다.
- `-d /home/newuser`: 홈 디렉터리를 /home/newuser로 지정합니다.
- `-s /bin/bash`: 사용자의 로그인 셸을 /bin/bash로 지정합니다.
- `-c "John Doe"`: 사용자 계정에 대한 설명을 "John Doe"로 설정합니다.
- `newuser`: 새로 생성될 사용자의 이름입니다.

이 명령어를 실행하면 newuser라는 사용자가 생성되고, 홈 디렉터리가 /home/newuser에 생성되며, wheel 그룹에 속하고,      
/bin/bash 셸을 사용하게 되며, 계정 설명이 "John Doe"로 설정된다.

## passwd
사용자 비밀번호를 설정하려면 passwd 명령어를 사용한다. passwd 명령어를 사용하여 특정 사용자의 비밀번호를 설정할 수 있다.
```linux
sudo passwd newuser
```

## more
파일 내용을 확인하는 명령어. 파일을 읽어 화면에 화면 단위로 끊어서 출력하는 명령어. 위에서 아래 방향으로 출력되기 때문에 
지나간 내용을 다시 볼 수 없다.
```linux 
more <파일명>

왼쪽 하단에 화면에 출력된 내용이 전체의 몇 % 인지를 표시하며,    
Enter 키를 입력하면 한 줄씩 출력되고 Space bar를 입력하면 한 화면씩 출력된다.
```
```linux
more -n <파일명>

-n 옵션을 사용할 경우, n에 입력한 값만큼 끊어서 화면에 출력해준다.   
Space bar를 입력하면 입력한 값 만큼씩 끊어서 화면에 출력된다.
```
```linux
more +n <파일명>

+n 옵션을 사용할 경우, n에 입력한 행부터 화면에 출력해준다.
```

## cp(Copy)
### file1을 file2라는 이름으로 복사
```linux
cp file1 file2
```
### 강제 복사
```linux
file2라는 파일이 이미 있을 경우 강제로 기존 file2를 지우고 복사 진행

cp -f file1 file2
```
### 디렉토리 복사
```linux
폴더 안의 모든 하위 경로와 파일들을 복사

cp -r dir1 dir2 
```

## mv(Move)
### file1 파일을 file2 파일로 변경
```linux
mv file1 file2
```
### file1 파일을 dir 디렉토리로 이동
```linux
mv file1 /dir
```
### 여러 개의 파일을 dir 디렉토리로 이동
```linux
mv file1 file2 /dir
```
### dir1 디렉토리를 dir2 디렉토리로 이름 변경
```linux
mv /dir1 /dir2
```

## rm(Remove)
### 파일 삭제
```linux
rm <파일>
```
### 파일 강제 삭제
```linux
삭제확인 과정을 거치지 않는다.

rm -f <파일>
```
### 디렉토리 내 모든 파일 삭제
```linux
/home/app 폴더 내 모든 파일 삭제

rm /home/app/*
```
### 디렉토리 삭제
```linux
app 폴더 삭제

rm -r /home/app
```
## 다중 명령어
### 세미콜론(;)
```linux
하나의 명령어 라인에서 여러 개의 명령을 실행

cp .profile new; cat new; haed -3 new; ls -al

cp .profile new : .profile 파일을 new 파일에 복사
cat new : new 파일을 출력
head -3 new : 다시 new 파일을 출력하는데 앞의 3줄만 출력
ls -al : 파일 목록 출력
```
### 파이프(|)
```linux
2개의 프로세스를 연결해주는 연결 통로 역할

ls -al | grep bash
```
### 더블 엔드퍼센트(&&)
```linux
다중 명령어를 한 라인에 실행할 수 있지만
첫 번째 명령이 에러 없이 정상적으로 종료했을 때만 두 번째 명령어 수행.
세미콜론(;)은 명령어가 정상적으로 수행해야 다음 명령어 수행.

more && printf "hello\n"
```
### 더블 버티컬바(||)
```linux
첫 번째 명령의 결과가 에러가 발생할 경우 뒤에 명령을 수행.
```
### 리다이렉션(<, >)
- 명령어 `>` 파일 `덮어쓰기`  
`명령어`에서 출력하는 것을 `파일` 이라는 파일에 기록. 에러 메시지는 파일로 저장되지 않는다.
```linux
[root@localhost tmp]# ls
01.txt 02.txt
[root@localhost tmp]# ls > ls_list.txt
[root@localhost tmp]# ls
01.txt 02.txt ls_list.txt
[root@localhost tmp]# cat ls_list.txt
01.txt
02.txt
ls_list.txt
```
- 명령어 `>>` 파일 `추가`  
꺽쇠가 두개 연달아 있는 것은 파일의 끝에 내용을 덧붙이라는 것이다.
```linux
[root@localhost tmp]# ls >> ls_list.txt
[root@localhost tmp]# ls
01.txt 02.txt ls_list.txt
[root@localhost tmp]# cat ls_list.txt
01.txt
02.txt
ls_list.txt
01.txt
02.txt
ls_list.txt
```
- 명령어 `<` 파일      
파일로부터 입력을 받는다.
```linux
[root@localhost tmp]# tail -n 2 < ls_list.txt
02.txt
ls_list.txt

tail 명령어는 마지막 행을 출력하는 명령어다. 그리고 -n 2 옵션은 마지막 2개 행을 출력하겠다는 의미이다. 
따라서, ls_list.txt 파일을 입력받아 마지막 2개 행을 출력하라는 의미다.
```

### kill
```linux
-9 : 
리눅스 커널이 프로세스를 강제 종료한다.
작업중인 모든 데이터를 저장하지 않고 프로세스를 종료하기 때문에
저장되지 않은 데이터는 소멸된다.

kill -9 'PID'

-15 : 
TERM 시그널 (기본 옵션)
자신이 하던 작업을 모두 안전하게 종료하는 절차를 밟는다.
메모리상에 있는 데이터와 각종 설정/환경 파일을 안전하게 저장한 후 프로세스를 종료

kill -15 'PID'
```

## tail
tail 명령어는 파일의 마지막 행을 기준으로 지정한 행까지의 파일 내용 일부를 출력해주는 명령어이다. 기본값으로는 마지막 10줄을 출력하며 주로 tail은 리눅스에서 오류나 파일 로그를 실시간으로 확인할 때 매우 유용하게 사용된다.
- 자주 사용하는 옵션
  - `-f` : tail을 종료하지 않고 파일의 업데이트 내용을 실시간으로 계속 출력한다.
  - `-n (라인 수)` : 파일의 마지막줄부터 지정한 라인수까지의 내용을 출력한다.
  - `-c (바이트 수)` : 파일의 마지막부터 지정한 바이트만큼의 내용을 출력한다.
  - `-q` : 파일의 헤더와 상단의 파일 이름을 출력하지 않고 내용만 출력한다.
  - `-v` : 출력하기전에 파일의 헤더와 이름 먼저 출력한 후 파일의 내용을 출력한다.
- 실시간 로그 보기 (tail + grep)
```linux
tail -f mylog.log | grep 192.168.15.86
```
파이프를 사용해서 다른 명령어를 조합해서 사용하실 수도 있다. 실시간 로그 체크를 할 때는 tail과 grep 명령어 조합으로 로그파일에서 자신이 원하는 키워드만 추출한다. 위의 명령어대로 사용하시면 mylog파일을 실시간으로 액세스하고 IP주소가 192.168.42.12인 행만 추출할 수 있다.
- 여러 파일을 동시에 표시하는 법
```linux
tail mylog1.log mylog2.log
```
tail명령어의 파일이 여러개를 입력하면 각 파일의 마지막 부분을 확인하실 수 있다.

## ssh
### ssh(secure shell) 옵션
|옵션|내용|비고|
|------|-----------------------|------------------------|
|-a|인증 에이전트의 전송을 불허||
|-c|세션을 암호화하는데 사용할 암호 해독기를 선택한다.|idea가 기본값이고 arcfour가 가장 빠르며, none은 rlogin이나 rsh(암호화 없음)을 사용|
|-e|세션에 대한 이스케이프 문자를 설정||
|-f|인증과 전송이 설정된 후에 백그라운드에서 ssh를 설정||
|-i|RSA 인증을 위한 비밀 키를 읽어 올 아이덴티티 파일을 선택||
|-k|Kerberos 티켓의 전송을 불허||
|-l|원격 시스템에 사용할 로그인 이름을 설정|예시) ssh -i ec2-private-seoul.pem ec2-user@private-ec2-a1인스턴스프라이빗IP주소|
|-n|ssh가 백그라운드에서 실행될 때 사용되는 /dev/nulls 로 부터 의 stdin을 재지정||
|-o|구성 파일의 형식을 따르는 사용자 정의 옵션 사용||
|-p|원격 호스트에 있는 연결할 포트 설정||
|-q|모든 메시지를 억제하는 quiet 모드를 활성화한다.||
|-P|특권이 부여되지 않은 포트를 사용||
|-t|pseudo -tty 할당을 강제.||
|-v|(디버깅에 유용한) 자세한 정보 표시 모드를 활성화||
|-x|X11 전송을 불가능하게 한다.||
|-C|모든 데이터의 압축을 요구한다.||
|-L|지정된 원격 호스트와 포트에 전송할 로컬 포트 설정||
|-R|로컬 호스트와 지정된 포트로 전송될 원격 포트 설정||

## su와 sudo
### sudo (Super User Do)
sudo 명령어는 특정 명령을 슈퍼유저(또는 다른 사용자)의 권한으로 실행할 수 있게 해준다. sudo 는 현재 사용자의 환경을 유지하면서 필요한 권한만 잠시 얻어 명령을 실행할 수 있는 방식이다.

```linux
sudo <command>
```

#### sudo 예제
```linux
# 패키지 설치
sudo apt-get install package_name

# 파일 편집
sudo nano /etc/hosts

# 시스템 서비스 재시작
sudo systemctl restart apache2
```

### su (Substitute User)
su 명령어는 현재 세션을 다른 사용자로 변경하는 데 사용됩니다. 기본적으로 su 는 루트 사용자로 전환하지만, 다른 사용자로 전환할 수도 있습니다.

```linux
su [username]
```

#### 예제
```linux
# 루트 사용자로 전환
su -

# 다른 사용자로 전환
su username

# 명령어를 실행하고 원래 사용자로 돌아오기
su -c "command" username
```

### 차이점
|기능|`sudo`|`su`|
|-----|-----|-----|
|목적|단일 명령어에 대해 권한 상승|다른 사용자로 전환|
|비밀번호|현재 사용자의 비밀번호를 입력|전환할 사용자의 비밀번호를 입력|
|환경|현재 사용자의 환경을 유지|전환된 사용자의 환경을 로드|
|지속성|단일 명령어에 대해서만 권한 상승|새로운 셀을 열고 지속적으로 사용|
|로그 기록|모든 `sudo` 명령어가 로그에 기록됨|`su` 자체는 로그에 기록되지 않음|
