# (Day 2) RAID

### 리눅스

---

- **상대 경로, 절대 경로**
    1. 상대 경로 (.) : 사용자의 현재 위치에 따른 접근 경로
    2. 절대  경로(/) : 어떤 위치라도 동일한 접근 경로

- 홈 디렉토리 이동
    1. `cd`
    2. `cd~`
    3. `cd ..` 
    4. `cd /root`
    5. `cd$HOME`

- mv 명령어 : 파일 옮길 때
    
    - `mv aaa bbb ccc ddd(디렉토리)` aaa, bbb, ccc 파일을 ddd디렉토리 안으로 이동
    
    >  1. `[출력] > [파일]`  :  출력결과를 파일에다가 계속 덮어 쓸 때
      2. `[출력] >> [파일]` :  append (계속 쌓임, log)
- cron 명령 (/etc/crontab 파일)
    - `분 시 일 월 요일 사용자 명령어`
    - `20 3 16 * * root /root/backup.sh`
    (매월 16일 새벽 3시 20분에 /home 디렉토리 전체를 백업해서 /backup 디렉토리에 저장)

- RAID : 여러 개의 하드디스크를 하나의 하드디스크처럼 사용하는 방식

    ![Day%202%20RAID/Untitled.png](images/Day%202%20RAID/Untitled.png)

    1. Lenear RAID
        - 최소 2개의 HDD 필요
        - 2개 이상의 HDD를 1개의 볼륨으로 사용
        - 앞 디스크부터 차례로 저장
        - 100% 공간효율성 = 저렴한 비용

            ![Day%202%20RAID/Untitled%201.png](images/Day%202%20RAID/Untitled%201.png)

    2. RAID 0 (= Stripping)
        - 최소 2개의 HDD 필요
        - 모든 디스크에 동시 저장
        - 100% 공간 효율성 = 저렴한 비용
        - 낮은 신뢰성 (⇒ 빠른 성능 요구 & 손실되어도 무관한 데이터 저장에 적합)

            ![Day%202%20RAID/Untitled%202.png](images/Day%202%20RAID/Untitled%202.png)

    3. **RAID 1 (=Mirroring)****
        - 결함 허용(Fault-tolerance) = 높은 신뢰성 (장애 발생 시 복구 가능)
        - 데이터 저장에 2배의 용량 필요 = 나쁜 공간효율성 = 높은 비용
        - 저장 속도(성능)은 변함없음
        - 중요 데이터 저장에 적합

            ![Day%202%20RAID/Untitled%203.png](images/Day%202%20RAID/Untitled%203.png)

    4. **RAID 5 = RAID0 + 1 Parity*** (Parity : 데이터를 검증하는 값)
        - RAID0의 공간 효율성 + RAID1의 데이터 안정성
        - 최소 3개 이상의 HDD 필요
        - 공간 사용 : 전체 HDD개수 - 1
        - 오류가 발생 시, 패리티(Parity)를 이용해서 데이터를 복구

            ![Day%202%20RAID/Untitled%204.png](images/Day%202%20RAID/Untitled%204.png)

        - HDD 2개가 동시에 문제가 생기면, 복구 불가

            ![Day%202%20RAID/Untitled%205.png](images/Day%202%20RAID/Untitled%205.png)

    5. RAID6 (=RAID 0 + 2 Parity)
        - RAID5을 개선
        - 최소 4개 이상의 HDD가 필요
        - RAID5보다 공간효율성, 성능 떨어짐

            ![Day%202%20RAID/Untitled%206.png](images/Day%202%20RAID/Untitled%206.png)

    6. RAID 1+0
        - 신뢰성과 성능(속도)을 보장

            ![Day%202%20RAID/Untitled%207.png](images/Day%202%20RAID/Untitled%207.png)

    ### 실습 - RAID 구축

    ---

    - 하드웨어 구성도
    1. **9개의 하드디스크 추가**

        ![Day%202%20RAID/Untitled%208.png](images/Day%202%20RAID/Untitled%208.png)

    2. **서버를 부팅 후, 추가된 하드디스크 (가상머신 우측 하단) 9개를 확인**

        ![Day%202%20RAID/Untitled%209.png](images/Day%202%20RAID/Untitled%209.png)

    3. **터미널에서 추가된 장치 (`/dev/sd*`)**

        ![Day%202%20RAID/Untitled%2010.png](images/Day%202%20RAID/Untitled%2010.png)

    4. `**fdisk` 명령으로 9개의 하드디스크를 RAID용 파티션으로 만들기**
        - 파일시스템 : `fd` (Linux radi autodetect)
        1. `fdisk /dev/DISK_NAME`
        2. Command : `n` ← 새로운 파티션 분할
        3. Select : `p` ← Primary 파티션 선택
        4. Partition number : `1` ← 파티션 번호는 1번으로 선택
        5. Command : `t` ← 파일시스템 유형 선택
        6. Hex Code : `fd` ← 'Linux raid autodetect' 유형 번호
        7. Command : `p` ← 설정 내용 확인
        8. Command : `w` ← 설정 저장

    5. mdadm 패키지 설치 
    `apt-get -y install mdadm`
        - root@swarm-manager:~# mdadm --help

            mdadm is used for building, managing, and monitoring

            Linux md devices (aka RAID arrays)

    6. `mdadm` 명령어를 사용하여 실제 RAID를 구성 (Linear RAID)

        mdadm : **RAID를 생성 및 관리하는 명령어**

        - root@swarm-manager:~# `mdadm --create /dev/md9 --level=linear --raid-devices=2 /dev/sdb1 /dev/sdc1`
        mdadm: Defaulting to version 1.2 metadata
        mdadm: array /dev/md9 started.
        - root@swarm-manager:~# `mdadm --detail --scan`  (생성된 RAID 확인)
        ARRAY /dev/md9 metadata=1.2 name=swarm-manager:9 UUID=79277354:d0d2d775:b5fe32a0:f85bd2bc
        - 생성한 파티션 장치의 파일 시스템을 생성 (= 포맷)
        root@swarm-manager:~# `mkfs.ext4 /dev/md9`
        - 마운트할 디렉토리 생성 후, 파일 시스템을 마운트
        `mkdir /raidLienar
        mount /dev/md9  /raidLinear`

        (마운트 정보를 확인) `df`

            ![Day%202%20RAID/Untitled%2011.png](images/Day%202%20RAID/Untitled%2011.png)

        - 컴퓨터가 언제든지 마운트하도록 설정 (`gedit /etc/fstab`)

        (아래 내용을 추가)
        /dev/md9  /raidLinear  ext4  details  0  0
    7. RAID 0, 1, 5를 같은 방법으로 생성
    - **생성한 모든 RAID를 조회**
    `df | grep /dev/md`

        ![Day%202%20RAID/Untitled%2012.png](images/Day%202%20RAID/Untitled%2012.png)

    **다른 사이즈로 미러링을 한다면?** 

### 실습 2 - RAID에서의 장애 복구

---

1. **테스트 용도의 파일을 각 RAID 장치에 생성**

    root@swarm-manager:~# `cp /boot/vmlinuz-4.4.0-21-generic /raidLinear/testFile`
    root@swarm-manager:~# `cp /boot/vmlinuz-4.4.0-21-generic /raid0/testFile`
    root@swarm-manager:~# cp /boot/vmlinuz-4.4.0-21-generic /raid1/testFile
    root@swarm-manager:~# cp /boot/vmlinuz-4.4.0-21-generic /raid5/testFile

2. **시스템 종료** `halt -p`
3. **4개의 하드디스크를 고장 (제거)**  / 2, 4, 6, 9

    ![Day%202%20RAID/Untitled%2013.png](images/Day%202%20RAID/Untitled%2013.png)

    ![Day%202%20RAID/Untitled%2014.png](images/Day%202%20RAID/Untitled%2014.png)

4. 부팅 오류 발생

    ![Day%202%20RAID/Untitled%2015.png](images/Day%202%20RAID/Untitled%2015.png)

5. root 계정으로 접속하여 하드디스크 확인 (⇒ 제거된 하드디스크 확인)
`ls -l /dev/md*`

6. 고장난 하드디스크를 가동하고, 동작여부를 확인

    ![Day%202%20RAID/Untitled%2016.png](images/Day%202%20RAID/Untitled%2016.png)

    ![Day%202%20RAID/Untitled%2017.png](images/Day%202%20RAID/Untitled%2017.png)

- 9(LinearRAID), 0(RAID 0)에 경우는 결합 허용이 불가하여, 가동해도 적용이 되지 않음)

    ![Day%202%20RAID/Untitled%2018.png](images/Day%202%20RAID/Untitled%2018.png)

    1. LinearRAID와 RAID0을 중지
    `mdadm —stop /dev/md9`
    `mdadm —stop /dev/md0`
    2. `vi /etc/fstab` 에서 LinaerRAID와 RAID 부분을 주석 처리(#)
    3. reboot

- RAID 5와 1의 정상 동작을 확인

    ![Day%202%20RAID/Untitled%2019.png](images/Day%202%20RAID/Untitled%2019.png)

7.