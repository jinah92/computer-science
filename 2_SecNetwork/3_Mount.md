# (Day 3) Mount

### 마운트란?

- 물리적인 장치를 특정 위치(대부분 폴더)에 연결시켜 주는 과정

### <실습 1> CD/DVD 마운트

### 1) 기존 마운트 정보 확인

---

1. (root사용자) 터미널에서 `mount` 입력 ⇒ 현재 마운트된 장치들을 확인

    ![Day%203%20Mount/Untitled.png](images/Day%203%20Mount/Untitled.png)

### 2) 서버에 CD/DVD 넣기

---

1. CD/DVD 마운트 해제  `umonut dev/cdrom`

    ![Day%203%20Mount/Untitled%201.png](images/Day%203%20Mount/Untitled%201.png)

2. VMware에 CD/DVD를 넣기
VM → Settings → CD/DVD에서 [connected]와 [Connect at power on]체크 → [Use ISO image file:] 선택후 Ubuntu Desktop 16.04 LTS DVD ISO 파일 선택
3. 터미널에서 `mount` 명령으로 결과를 확인

    ![Day%203%20Mount/Untitled%202.png](images/Day%203%20Mount/Untitled%202.png)

### 3) 서버에 마운트된 CD/DVD 사용

---

1. DVD 패키지가 들어있는 디렉토리로 이동
`cd /media/root/Ub (탭키)`  ← 디렉토리 이동
`pwd`                                    ← 현재 디렉토리 위치 확인
`ls`

    ![Day%203%20Mount/Untitled%203.png](images/Day%203%20Mount/Untitled%203.png)

2. DVD 안의 파일 확인
    - casper 디렉토리로 이동
    - casper 디렉토리 안의 filesystem.squash 파일 ⇒ 우분투 전체가 들어있는 파일 (즉, 우분투 설치하면 해당 파일의 압축이 풀리며 전체 시스템이 구성)

    ![Day%203%20Mount/Untitled%204.png](images/Day%203%20Mount/Untitled%204.png)

- DVD를 더 이상 사용하지 않음 `umount /dev/cdrom`
⇒ target is busy (현재 작업 중이 디렉토리가 DVD가 마운트된 /media/root/의 하위 디렉토리)
⇒ (해결) `cd /media` 로 원래 디렉토리에 이동 후 다시 입력

    ![Day%203%20Mount/Untitled%205.png](images/Day%203%20Mount/Untitled%205.png)

    **DVD의 마운트 해제**
    ⇒ 현재 마운트된 디렉토리에서 실행하지 않는다

### 4) Client에서 USB 메모리를 사용하기

---