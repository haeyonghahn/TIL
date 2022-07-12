# Linux 명령어
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
