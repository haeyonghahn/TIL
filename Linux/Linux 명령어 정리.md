# Linux 명령어
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
