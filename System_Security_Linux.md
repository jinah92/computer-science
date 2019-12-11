# (Day 5-6) 시스템 보안 (리눅스)

## **ACL (Access Control List)**

- MAC (Mandantory Access Control)
- DAC (Dictionary Access Control)
- RBAC (Role-based Access Control)

# 리눅스 계정, 권한 체계

---

## /etc/password

- 계정 목록을 저장하고 있는 파일

![Day%205%206/Untitled.png](Day%205%206/Untitled.png)

## UID(UserID), GID(GroupID)

- 관리자 '0' 으로, 일반 사용자는 그 외 번호로 설정
- UID가 동일하면 사용자 계정이 달라도 동일한 권한을 가짐

## 파일 접근 권한

---

![Day%205%206/Untitled%201.png](Day%205%206/Untitled%201.png)

[파일 접근 권한 ①](https://www.notion.so/5d972cdfd26946d388fcf551f6351481)

## 파일 및 디렉토리 기본 권한

- 기본 생성 최고 권한에서 umask를 뺀 값
- 기본 생성 최고 권한 : 666(파일), 777(디렉토리)
- umask 설정 : `/etc/profile` 파일

### umask 설정 (실습)

- `getid /etc/profile` (/etc/profile 파일 수정)
- 파일 하단에 umask 022 입력 후, 저장

![Day%205%206/Untitled%202.png](Day%205%206/Untitled%202.png)

- 디렉토리, 파일 생성
`mkdir dir-umask-022`      umask-022 디렉토리 생성
`touch txt-umask-022`      umask-022 txt 파일 생성
- `ls -al`        현재 디렉토리의 파일 확인
    - txt-umask-022 (**644** ← 666 - 022)
    - umsk-022     (**755** ← 777 - 022)

![Day%205%206/Untitled%203.png](Day%205%206/Untitled%203.png)

## 계정 관리

---

### shadow 패스워드

- /etc/passwd 파일은 일반 사용자가 읽을 수 있으므로 위험함 ⇒ 계정정보의 노출 위험
- 암호화한 패스워드와 유효기간 등을 /etc/shadow 파일에 저장, root계정만 읽도록 제한
- 패스워드에 대한 보안 정책 적용이 가능
- 임의 생성된 관리자 계정의 존재 여부를 확인 ⇒ 관리자 계정은 root 외의 다른 이름을 사용하도록 함

![Day%205%206/Untitled%204.png](Day%205%206/Untitled%204.png)

### 서버 이전시, 계정정보와 패스워드의 안전한 보호

- `pwconv`
    - shadow 패스워드 정책을 적용하도록 하는 명령어
    - /etc/passwd 파일의 두번째 필드에 있는 암호화된 패스워드를 모두 x로 표시 후, /etc/shadow 의 두번째 필드에 저장
    - 패스워드 사용 문자 수, 사용 기간 설정 등의 shadow 패스워드 정책 적용
    - 현재 리눅스 버전에서 거의 필요하지 않음 (이미 대부분 shadow 패스워드 정책을 적용)
- `pwunconv`
    - shadow 패스워드 정책 → 일반 패스워드 정책으로 변경

### 현재 디렉토리 확인

- `pwd`

### 파일 데이터 출력 (cat)

- `cat /etc/passwd | grep root` 
- etc/passwd 파일 내용 중, root에 해당한 데이터만 출력

### 유저 계정 생성

- `adduser '유저명'`

### su, sudo 명령어

- **su - '유저명':** 이후 모든 명령어를 해당 유저의 권한으로 실행 (root 권한 실행 가능)
- **sudo :** 해당 명령어 실행동안만 root 권한(super user)을 가지고 해당 명령어를 실행

### 시스템에 로그인할 필요가 없는 사용자는 '쉘 제거'

- /ect/passwd 파일에서 쉘이 정의된 부분을 수정 또는 `usermod` 명령어를 이용하여 쉘 정보를 변경
- 웹 사용자 중 시스템에 로그인할 필요가 있는 사용자 ⇒ 쉘이 부여된 별도의 계정을 사용

[/bin/false 과 /sbin/nologin](https://www.notion.so/45c76c94f2f2412abd284f6d0fa42e56)

![Day%205%206/Untitled%205.png](Day%205%206/Untitled%205.png)

- 윈도우에서 telnet으로 Kali#1에 접근 
> `telnet Kali#1의 ip` 
> 계정 입력 : user11
    - /bin/false인 경우 ⇒ 메시지 출력없이 연결 해제
    - /usr/sbin/nologin 인 경우 ⇒ 메시지 출력 후 연결 해제

![Day%205%206/Untitled%206.png](Day%205%206/Untitled%206.png)

쉘이 없는 사용자에게는 접근 불가

## 권한 상승 (SetUID)

---

- 유닉스 시스템 해킹의 중요 요소
- **rwsr-xr-x 로 설정된 경우
⇒ 파일의 소유자가 아니어도,  '파일 소유자'의 권한으로 해당 파일을 실행 (권한 상승)**
- s : 4000
- SetUID 파일을 삭제한다 (권장)
- `find / -user root -perm /4000`  파일 소유자가 root이면서 setuid 비트를 가진 파일 찾기

![Day%205%206/Untitled%207.png](Day%205%206/Untitled%207.png)

- **SetUID 설정 (실습)**
    - /test/dash 파일 생성 (복사)  `cp /bin/dash /test/dash`

    ![Day%205%206/Untitled%208.png](Day%205%206/Untitled%208.png)

    - setuid 설정 `chmod 4xxxx '파일명'`

    ![Day%205%206/Untitled%209.png](Day%205%206/Untitled%209.png)

    - user11에서 해당 파일 실행 **프롬프트 ⇒ # (관리자 권한 획득)**

    ![Day%205%206/Untitled%2010.png](Day%205%206/Untitled%2010.png)

- **SetUID 설정 - 프로그래밍 (실습)**
    - 소스코드 파일(.c) 컴파일

    ![Day%205%206/Untitled%2011.png](Day%205%206/Untitled%2011.png)

    - backdoor 파일 확인

    ![Day%205%206/Untitled%2012.png](Day%205%206/Untitled%2012.png)

## 패스워드 복잡도

---

### 크래킹되기 쉬운 패스워드

### 크래킹되지 어려운 패스워드

- 영문자 + 숫자 + 특수문자 조합

### john the ripper

- root 계정에서 john-ther-ripper 압축파일 설치
`cd /apt`
`wget '압축파일 주소'`

![Day%205%206/Untitled%2013.png](Day%205%206/Untitled%2013.png)

- 압축 파일 해제
`tar xvf '압축 파일명'`

![Day%205%206/Untitled%2014.png](Day%205%206/Untitled%2014.png)

- 압축 해제된 파일 중, src 디렉토리로 이동

![Day%205%206/Untitled%2015.png](Day%205%206/Untitled%2015.png)

- run 디렉토리 이동

![Day%205%206/Untitled%2016.png](Day%205%206/Untitled%2016.png)

- /etc/shadow파일을 /etc/passwd로 바꾼 후, myfile로 저장
`./unshadow /etc/passwd /etc/shadow > myfile`
- myfile 실행 (크래킹)
`./john myfile`

![Day%205%206/Untitled%2017.png](Day%205%206/Untitled%2017.png)

## 모바일

---

### 안드로이드 앱의 보안 요구사항

1. rooting이 되지 않게
2. 난독화
3. 권한 설정 파일 `app.manifest`
4. 서명 = 앱의 소유권
5. 안드로이드 권한 가이드
[https://developer.android.com/guide/topics/security/permissions.html?hl=ko](https://developer.android.com/guide/topics/security/permissions.html?hl=ko)
[https://developer.android.com/guide/topics/permissions/overview?hl=ko](https://developer.android.com/guide/topics/permissions/overview?hl=ko)

![Day%205%206/Untitled%2018.png](Day%205%206/Untitled%2018.png)