# 윈도우10 도커 설치시 WSL 2 installation is incomplete 에러 해결

![image](https://user-images.githubusercontent.com/31242766/188198235-e08a01bd-3de9-45b2-8c11-f1d7f48e4271.png)

윈도우에서 도커를 설치하다가 WSL2가 설치되지 않았다는 오류 메세지가 뜨면, 리눅스 커널 업데이트를 해야 한다.

#### 1. 파워쉘을 관리자 권한으로 실행

#### 2. 리눅스 서브시스템 활성 명령어 입력     
`dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`

![image](https://user-images.githubusercontent.com/31242766/188198728-aa5b030c-1d28-4453-abe6-e61849a52265.png)

#### 3. 가상 머신 플랫폼 기능 활성화 명령어 입력
`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`

![image](https://user-images.githubusercontent.com/31242766/188199243-679431a3-4003-42fa-a7a3-fbeb06255afc.png)

#### 4. x64 머신용 최신 WSL2 Linux 커널 업데이트 패키지 다운로드, 설치
wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

![image](https://user-images.githubusercontent.com/31242766/188199624-f76403fe-e171-4bcd-9180-843c906cbc32.png)
