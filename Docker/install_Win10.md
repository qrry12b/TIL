# 윈도우10 Docker 설치

1. WSL 옵션 기능을 사용하도록 설정
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

- - - - -
2. WSL 2 실행을 위한 요구 사항 확인
    - x64 시스템의 경우: 버전 1903 이상, 빌드 18362 이상
    - ARM64 시스템의 경우: 버전 2004 이상, 빌드 19041 이상

- - - - -
3. VirtualMachinePlatform 기능을 사용하도록 설정
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

- - - - -
4. 최신 WSL2 Linux 커널 업데이트 패키지 설치 (둘다 설치하게 아닌 종류에 맞춰 하나만 설치합니다)   
\[ [x64 머신](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) / [ARM64 머신](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi) \]

- - - - -
5. WSL 2를 기본 버전으로 설정
wsl --set-default-version 2

- - - - -
6. Docker 웹페이지에서 설치 파일 다운로드 및 설치   
[LINK](https://www.docker.com/products/docker-desktop)

- - - - -
```
WSL 2 installation is incomplete.

The WSL 2 Linux kernel is now installed using a separate MSI update package. Please click the link and follow the instructions to install the kernel updatte : https://aka.ms/wsl2kernel.

Press Restart after installing the Linux kernel.
```
만약 WSL 2 설치 전에 Docker를 먼저 실행해서 다음과 같은 메시지만 계속 출력되거나 실행에 실패한다면 트레이 아이콘에 있는 Docker를 종료 후 다시 실행하거나 재부팅 해주세요.

- - - - - 
### REF
* [이전 버전 WSL의 수동 설치 단계](https://docs.microsoft.com/ko-kr/windows/wsl/install-manual)
* [Docker Desktop for Mac and Windows | Docker](https://www.docker.com/products/docker-desktop)