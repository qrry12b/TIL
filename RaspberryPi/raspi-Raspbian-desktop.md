# 라즈베리파이 - 라즈비안 OS 설치하기 (Desktop)

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.19)   

집에있는 라즈베리파이3 Model B+로 진행했다.

1. Micro SD Card 포맷

SD Card Formatter 프로그램으로 Low Format 진행.

2-1. 라즈베리파이 공식 사이트

2-2. 공식 사이트 다운로드의 Windows 용 Raspberry Pi Imager 실행

(Raspberry Pi Imager 뿐 아니라 balenaEtcher 또는 Win32 Disk Imager라는 프로그램을 사용하기도 하지만 편하게 전용 공식 툴을 사용)

2-3. CHOOSE OS 는 Operating System 즉, OS 를 선택한다.

본인은 Raspberry Pi OS Full(32-bit) 를 선택.

2-4. SCHOOSE SD CARD 는 설치할 SD Card 를 선택한다.

혹시라도 잘못선택하지 않도록 확인한다.

3. WRITE 클릭 시 모든 기존 데이터가 삭제된다는 알림이 표시된다.

Yes 를 선택하면 기존 데이터가 삭제되고 OS 설치가 시작된다.

3-1 Writing... 이 0% 에서 멈춰 있어도 포맷 & 이미지 다운로드 시간이라 생각하고 기다리면 나중에 천천히 증가한다. (만약 이미 이미지 파일이 있다면 Use custom 으로 선택할 수 있다)

3-2 Writing 작업 후 Verifying 이 진행된다.

3-3 Write Successful 이 출력되면 SD Card를 리더에서 제거하면 된다.

4. 설치된 SD Card와 라즈베리파이를 부팅한다. (만약 화면이 표시되지 않는다면 전력 부족을 의심하고 전원을 제외한 다른 연결을 제거 후 부팅하고 다시 연결해 보자)

4-1. Next 를 반복하면 국가 선택에서 South Korea 를 선택한다.

4-2. Change Password 에서 사용할 비밀번호를 입력한다.

4-3. Set Up Screen 에서 주변에 검은 테두리가 있다면 체크박스를 선택한다.

4-4. Select Wireless Network 에서 와이파이를 선택한다. 이미 유선으로 연결되어 있거나 무시하려면 Skip 이 가능하다.

4-5. Update Software 에서 Next로 소프트웨어 업데이트를 혹은 Skip 으로 넘길 수 있다.

4-6. Setup Complete 에서 Restart 를 선택해 재부팅 한다.

먼저 재부팅을 완료하면 언어가 깨진다. 

메뉴의 Raspberry Pi Configuration - Interfaces - SSH 를 Enable 로 설정하고 키보드 마우스 연결을 제거한다.

연결을 제거하기 전에 미리 라즈베리파이의 아이피 주소를 확인해 두면 편하다.

윈도우10 의 경우 선택적 기능 추가에서 OpenSSH 클라이언트를 추가할 수 있다.

설치 후 command 에서 ssh 계정명@아이피주소를 입력해 연결 할 수 있다.

sudo apt-get update

sudo apt-get upgrade

OS 설치 후 입력하면 몇몇 패키지에 새 버전이 있다.

sudo apt-get install fonts-unfonts-core

sudo reboot

재부팅 후 정상적으로 한글이 표시된다.

sudo apt-get intall ibus ibus-hangul

im-config -n ibus

한글 입력기 설치 및 설정은 SSH만 사용한다면 필요 없을 수 있다.