# (Day 4) 원격지 시스템 관리_텔넷, SSH, VNC

## 8.1. 텔넷 서버

---

### 8.1.1. 개요

### 8.1.2. 텔넷 서버 구축

- 서버 구축 과정
    1. 텔넷 서버 설치  `apt-get install xinetd telnetd`
    2. 설정 파일(`/etc/xinetd.d/telnet`) 편집 
    3. 텔넷 전용 사용자 생성 `adduser`
    4. 텔넷 서비스 가동 `systemctl restart xinetd`
    5. 방화벽 설정(포트 열기) `ufw allow 23/tcp`
    6. 클라이언트에서 접속 `telnet [서버 IP]`

### 실습 - 리눅스에 텔넷 서버를 설치 및 가동, 원격지의 Windows에서 리눅스 관리

- (Server) 텔넷 서버 패키지가 설치되었는지 확인
    - `dpkg -l xinetd` (설치 확인)
    - `apt-get install xinetd telnetd` (패키지 설치)

        ![Day%204%20_%20SSH%20VNC/Untitled.png](images/Day%204%20_%20SSH%20VNC/Untitled.png)

    - 텔넷 서버가 가동되도록 설정

        `/etc/xinetd.d` 폴더로 이동하여 빈 파일 생성 `touch telnet`

        ![Day%204%20_%20SSH%20VNC/Untitled%201.png](images/Day%204%20_%20SSH%20VNC/Untitled%201.png)

        telnet 파일을 열고, 아래 내용을 삽입 (`vi` 또는 `gedit`)

    servie telnet{
    	disable = no
    	flags = REUSER
    	socket_type = stream
    	
    }

포트(Port)
⇒ 가상의 논리적인 통신 연결 번호 (TCP 포트 또는 UDP 포트)

우분투는 기본적으로 root 사용자로 텔넷 접속을 허용하지 않는다!

## OpenSSH 서버

---

- 텔넷과 용도가 동일하나, 보안이 강화된 서버 **(데이터 전송 시 암호화)**
- 서버 구축 과정
    1. ssh 서버 설치 및 가동 `apt-get install openssh-server`  , `systemctl restart ssh`
    2. 방화벽 설정 `ufw allow 22/tcp`
    3. 클라이언트 접속
    (리눅스) `ssh 사용자명@서버IP`
    (윈도우) Putty로 접속

### 실습 - 리눅스에 OpenSSH 서버 구축

(Server)

- 관련 패키지 설치  `apt-get -y install openssh-server`
- 서비스를 재가동/상시가동/가동확인
    - `systemctl restart ssh` →  서비스 재가동
    - `systemctl enable ssh` → 서비스 상시가동
    - `systemctl status ssh` →  서비스 가동 여부 확인(Active/running 확인)
- 방화벽 설정 (TCP/22번)
    - `ufw allow 22/tcp`

(Client)

- Server의 SSH 서버에 접속
    - `ssh 사용자이름@호스트이름(또는 서버 IP주소)`
- 접속 종료
    - `exit`

## VNC 서버

---

- DD
- 서버 구축 과정
    1. VNC 서버 설치 `apt-get install vnc4server`
    2. 서버 설정 파일 수정 `xstartup`
    3. VNC 서비스 가동 `vncserver : 1`
    4. 방화벽 설정 `ufw allow 5901/tcp`
    5. vnc 클라이언트 설치
        - (리눅스) `apt-get install xtightvncviewer`
        - (윈도우) [www.tigervnc.com](http://www.tigervnc.com) 에서 다운로드

### 실습 - X 윈도우 환경으로 원격 접속을 지원하는 VNC 서버를 사용