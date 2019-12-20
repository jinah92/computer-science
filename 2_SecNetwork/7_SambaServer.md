# (Day 7) Samba 서버 설치 및 운영

## 개요

---

- 서로 다른 운영체제 사이의 자원을 공유하기 위해 개발된 서버
- Samba를 사용하는 방법
    1. 리눅스/유닉스에서 Windows의 자원을 사용
    2. Windows에서 리눅스/유닉스의 자원을 사용

**Samba 서버를 별도로 설치할 필요 없음!!**

## 실습 1 - Windows의 폴더를 공유, 리눅스에 접근하여 사용

---

### 실습 구성

- WinClient **⇒ Samba 서버**
    1. 자신의 **자원을 사용할 사용자를 추가**
    2. 자신의 **자원을 공유**
- Server **⇒ Samba 클라이언트**
    1. Samba 클라이언트 패키지가 설치되어 있는지를 확인
    2. smbclient 명령으로 **WinClient가 제공하는 자원을 확인**
    3. smbmount 명령으로 **WinClient가 제공한 공유 폴더를 마운트**

### WinClient (파일 생성 및 공유 설정)

- C:\sambaShare 폴더를 생성
- 해당 폴더를 [우클릭]-[속성]-[공유] 탭

    ![Day%207%20Samba/Untitled.png](images/Day%207%20Samba/Untitled.png)

- 사용자/사용권한 추가 (Everyone - 읽기/쓰기)

    ![Day%207%20Samba/Untitled%201.png](images/Day%207%20Samba/Untitled%201.png)

### WinClient (리눅스의 사용자를 추가 및 비밀번호 지정)

- 관리자 권한으로 명령 프롬포트 실행
- 현재 등록된 유저 확인 `net user`

    ![Day%207%20Samba/Untitled%202.png](images/Day%207%20Samba/Untitled%202.png)

- root 유저 (비밀번호 1234)를 추가 후, 등록된 것을 확인 `net user root 1234 /add`

    ![Day%207%20Samba/Untitled%203.png](images/Day%207%20Samba/Untitled%203.png)

### **Server (WinClient에서 공유한 폴더를 사용)**

- 관련 패키지를 설치
`apt-get -y install samba-common smbclient cifs-utils`
- Windows에서 공유한 폴더 및 프린터를 확인
`smbclient -L [Winclinet IP주소] -m SMB2`

![Day%207%20Samba/Untitled%204.png](images/Day%207%20Samba/Untitled%204.png)

- 

![Day%207%20Samba/Untitled%205.png](images/Day%207%20Samba/Untitled%205.png)

## 실습 2 - Windows에서 리눅스 폴더 및 프린터를 사용

---

### 실습 구성

- WinClient **⇒ Samba 클라이언트**
- Server **⇒ Samba 서버**

### Server (Samba 서버 환경 구축)

- 관련 패키지 설치
`apt-get -y install samba system-config-samba`
- 디렉토리를 공유
    - Samba 서버를 시작 및 상태 확인
    `systemctl restart smbd` ← Samba 서버를 시작
    `systemctl status smbd` ← Samba 서버의 상태 확인
    - 공유할 디렉토리생성 (/ubuntu_share)  후, 디렉토리 속성 변경
    `mkdir /ubuntu_share
    chmod 707 /ubuntu_share`
- Samba 공유 설정 (해당 실습은 보안을 고려하지 않음)
    - 빈 파일 생성 `touch /etc/libuser.conf`
    - Samba 서버 설정 창 활성화 `system-config-samba`

        ![Day%207%20Samba/Untitled%206.png](images/Day%207%20Samba/Untitled%206.png)

        ![Day%207%20Samba/Untitled%207.png](images/Day%207%20Samba/Untitled%207.png)

- 방화벽 비활성화 `ufw disable`
- Samba 서버 재구동 `systemctl restart smbd`