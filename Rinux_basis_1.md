# (Day 3) 리눅스 개념 및 명령어 - 1

Created: Dec 16, 2019 9:35 AM
Updated: Dec 16, 2019 5:43 PM

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

- **바탕화면**
[톱니바퀴 아이콘] ⇒ [로그아웃] 선택
- **텍스트 모드 (server B)**

logout 또는 exit