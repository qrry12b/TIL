# 라즈베리파이 - 라즈비안 OS 설치하기 (SSH)

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.23)   

Desktop 과정에서 SSH 연결을 통해 조금 다른 부분만 수정. (라즈베리파이3 Model B+로 진행)

1. Micro SD Card 포맷

 SD Card Formatter 프로그램으로 Low Format 진행.

2-1. 라즈베리파이 공식 사이트

2-2. 공식 사이트 다운로드의 Windows 용 Raspberry Pi Imager 실행

(Raspberry Pi Imager)

2-3. CHOOSE OS 는 Operating System 즉, OS 를 선택한다.

본인은 Raspberry Pi OS Life (32-bit) 를 선택. (가장 작은 용량)

2-4. CHOOSE SD CARD는 설치할 SD Card를 선택한다.

(혹시라도 잘못 선택하지 않도록 확인한다.

3. WRITE 클릭 시 모든 기존 데이터가 삭제된다는 알림이 표시된다.

Yes를 선택하면 기존 데이터가 삭제되고 OS 설치가 시작된다.

3-1 Writing... 이 0% 에서 멈춰 있어도 포맷 & 이미지 다운로드 시간이라 생각하고 기다리면 나중에 천천히 증가한다. (만약 이미 이미지 파일이 있다면 Use custom 으로 선택할 수 있다)

3-2 Writing 작업 후 Verifying 이 진행된다.

3-3 Write Successful 이 출력되면 SD Card를 리더에서 제거하면 된다.

3-4 Micro SD Card를 다시 연결 후 최상위에 ssh 라는 이름의 빈 파일을 추가한다.

3-5 wpa_supplicant.conf 파일을 최상위에 추가하고 내용을 작성한다.

(작성할때 network 안에 key="value" 앞에는 공백이 아닌 탭으로 한다)

(만약 그래도 문제가 있다면 공백 뒤로 있는 GROUP=netdev를 지운다)

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev

update_config=1

country=US

network={

        ssid="SSID"

        psk="PASSWORD"

}

4. 작성 후 SD Card를 라즈베리파이에 연결하고 부팅한다.

(조금 기다린 다음 SSH 연결을 시도한다)

4-1. 윈도우 10의 경우 선택적 긴능 추가에서 OpenSSH 클라이언트를 추가할 수 있으며 이하 버전에서는 별도의 프로그램 설치가 필요하다.

(네트워크 상에서 라즈베리파이 IP를 모른다면 arp- a 를 입력)

(라즈베리파이에서 iwconfig로 와이파이 연결 상태 확인 가능)

4-2. ssh 계정명@아이피주소로 연결한다. 만약 포트가 기본포트(22)가 아니라면 -p 옵션으로 지정할 수 있다.

(기본 계정 정보 : pi / raspberry)

(만약 WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED가 발생한다면 [ssh-keygen -R IP주소]를 통해 초기화 후 연결하면 해결된다)

4-3. sudo raspi-config
Interfacing Options-SSH를 enabled(활성) 한다.

Change User Password Change password for the 'pi' user 에서 비밀번호 변경

Localisation Options - Change Time Zone에서 표준시를 국내로 변경한다. (Asia-Seoul 선택)

Update(Update this tool to the latest version)을 한다.

5. 필요한 패키지 업데이트 & 설치

sudo apt-get update

sudo apt-get upgrade

sudo apt-get install fonts-unfonts-core

sudo reboot