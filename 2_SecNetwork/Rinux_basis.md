# (Day 3) 리눅스 개념 및 명령어 - 1

Created: Dec 16, 2019 9:35 AM
Updated: Dec 17, 2019 8:45 AM

### API 와 엔진의 차이

- API : 라이브러리를 응용해서 짠 나의 코드
- 엔진 : API에의 코드를 실행

### 구현체, 웹 컨테이너

- 구현체 (Implementation)

Application개발자:  interface를 통해 개발해야 한다

API 개발자: 자신이 짠 코드를 interface를 통해 어느 환경에서든지 개발해야 한다

### 숙제

---

- 마운트와 CD/DVD/USB의 활용 (p160 ~ p.174)
- 링크 (p.195 ~ p.197)
- 파일 압축과 묶기 (p.218 ~ p.220)
- 파일 위치 검색 (p.220 ~ p.222)

## 4. 1. 리눅스 운영 전에 알아야 할 개념

---

### 4.1.1. 시작과 종료

- **바탕화면**
[톱니바퀴 아이콘] ⇒ [컴퓨터 끄기] 실행

![Day%203%201/Untitled.png](images/Day%203%201/Untitled.png)

- **터미널/콘솔**

poweroff
shutdown -P now
halt -p
init 0

 * **시스템 종료 ⇒** -P 또는 -p 

- shutdown  ⇒ **now 부분에 시간 지정  = 지정한 시간에 시스템을 종료**
shutdown -P +10     
→ 10분 후에 종료 (P : Poweroff)

shutdown -r 22:00 
→ 오후 10시에 재부팅 (r : reboot)

shutdown -C    
→ 예약된 shutdown을 취소 (C : Cancel)

shutdown -k + 15
→ 현재 접속한 사용자에게 15분 후에 종료된다는 메시지를 전송 (실제로 종료는 안됨)

### 4.1.2. 시스템 재부팅

- **바탕화면**
[톱니바퀴 아이콘] ⇒ [다시 시작] 실행
- **터미널/콘솔**

reboot
shutdown -r now
init 6

### 4.1.3. 로그아웃

- 리눅스는 여러 명의 사용자가 동시에 접속해서 사용하는 '다중 사용자(multi-user) 시스템'
- **바탕화면**
[톱니바퀴 아이콘] ⇒ [로그아웃] 선택
- **텍스트 모드 (server B)**

logout 또는 exit

### 4.1.4. 가상 콘솔

- 가상 콘솔 = 가상의 모니터
- 우분투는 총 7개의 가상 콘솔을 제공함
- 단축키 : `Ctrl` + `Alt` + `F1` ~`F7`

### <실습 1> - shutdown 명령 실행 시, 다른 사용자에게 메시지 전달하는 것을 확인

---

1. 2번째 가상콘솔 (root 사용자) : 텍스트 모드의 2번째 가상 콘솔로 이동 (`Ctrl` + `Alt` + `F2`)

    ![Day%203%201/Untitled%201.png](images/Day%203%201/Untitled%201.png)

2. 3번째 가상콘솔 (우분투 사용자) : 텍스트 모드의 3번째 가상 콘솔로 이동 (`Ctrl` + `Alt` + `F3`)

    ![Day%203%201/Untitled%202.png](images/Day%203%201/Untitled%202.png)

3. 2번째 가상콘솔 (root 사용자) : 시스템 5분 후 종료 ( `shutdown -h 5` )

    ![Day%203%201/Untitled%203.png](images/Day%203%201/Untitled%203.png)

    - root 사용자 메시지

        ![Day%203%201/Untitled%204.png](images/Day%203%201/Untitled%204.png)

    - ubuntu 사용자 메시지

        ![Day%203%201/Untitled%205.png](images/Day%203%201/Untitled%205.png)

    - `shutdown -c`  (shutdown 명령 취소)

### <실습 2> - 다른 사용자에게 시스템 접속을 로그아웃하도록 유도

---

1. 2번째 가상콘솔 (root 사용자) : `shutdown -k +10` (10분 후 시스템 종료, 실제로는 종료되지 않음)

    ![Day%203%201/Untitled%206.png](images/Day%203%201/Untitled%206.png)

 2. 3번째 가상콘솔 (우분투 사용자) : 시스템이 종료된다는 메시지를 수신

![Day%203%201/Untitled%207.png](images/Day%203%201/Untitled%207.png)

### 4.1.5. 런레벨 (Run-level)

- 리눅스에서의 시스템이 가동되는 방법은 7가지 런레벨로 나눌 수 있다

[리눅스의 런레벨](https://www.notion.so/f364a2738cec4924b834bffa9ad91a41)

### <실습> - 시스템에 설정된 런레벨을 변경

---

1. 시스템에 기본으로 설정된 런레벨 확인

`ls -l /lib/systemd/system/default.target`

- 그래픽 모드로 부팅되도록 설정하는 graphical.target을 가리키고 있음

    ![Day%203%201/Untitled%208.png](images/Day%203%201/Untitled%208.png)

2. 텍스트 모드로 부팅되도록 런레벨을 변경후, 재부팅 (reboot)

(1) `ln -sf /lib/systemd/system/multi-user.target /lib/systemd/system/default.target`
 - 런레벨 3번(mult-user.tartget)으로 링크 파일을 생성

(2) `ls -l /lib/systemd/system/default.target`
 - 변경 사항을 확인

![Day%203%201/Untitled%209.png](images/Day%203%201/Untitled%209.png)

### 도움말 `man` + `<명령어>`