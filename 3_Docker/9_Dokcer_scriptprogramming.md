# (Day 9) Dokcer_쉘 스크립트 작성

### 실습 - 도커 이미지 크기 비교 (불필요한 명령어 구문에 따라)

---

**도커는 명령어 한줄마다 Layer가 쌓인다 (크기 증가)**

### CASE 1

---

- 메모리를 할당 및 파일을 제거하는 명령어를 추가 (`gedit Dockerfile`)

    FROM ubuntu                         // Ubuntu 베이스 이미지 가져오기
    
    RUN mkdir /echo   
    
    RUN fallocate -l 100m /echo/dummy   // echo 디렉토리 아래에 100MB 크기의 공간(/dummy)을 할당
    
    RUN rm /echo/dummy                  // dummy 공간을 제거

- `docker build -t falloc100m .`  (컨테이너 필드)
- 생성된 이미지 (우분투 이미지 - 169M)

![Day%209%20Dokcer_/Untitled.png](images/Day%209%20Dokcer_/Untitled.png)

### CASE 2 (권장)

---

- 파일 내용을 바꾼후 컨테이너의 이미지 크기를 비교 (최종 결과만을 Layer로 생성 - 100MB 크기의 파일을 생성 후에 파일 제거)

    FROM ubuntu
    
    RUN mkdir /echo && \
    fallocate -l 100m /echo/dummy && \
    rm /echo/dummy

![Day%209%20Dokcer_/Untitled%201.png](images/Day%209%20Dokcer_/Untitled%201.png)

## 1. Docker에 웹서버 구축

---

### (실습 1) 아파치 웹 서버 설치 후,  로컬에 있는 hello.html 파일을 컨테이너의 /var/www/html 디렉토리로 복사

1. **홈 디렉토리에서 /webserver(디렉토리) 생성** 
`cd ~ 
mkdir webserver
cd webserver`

2. **생성한 디렉토리에 Dockerfile 파일 생성**

        FROM ubuntu
        
        RUN apt-get update
        
        RUN apt-get install apache2 -y  // ##1
        
        ADD hello.html /var/www/html/   // ##2
        
        WORKDIR /var/www/html           // ##3
        
        RUN [ "/bin/bash", "-c", "echo hello2 >> hello2.html" ]  // ##4
        
        EXPOSE 80    // ##5
        
        CMD apachectl -DFOREGROUND  // ##6

    ##1 -y : docker build 과정에서 사용자 입력이 발생하면 오류로 처리하므로, 사용자 입력이 발생하지 않도록 하기 위한 옵션

    ##2 ADD, COPY : 호스트의 파일 또는 디렉토리를 이미지 내부로 복사

    COPY ⇒ 호스트의 로컬 파일만 복사 가능
    ADD ⇒ 호스트의 로컬 파일 뿐 아니라 외부 URL, tar 파일도 복사 가능 (tar 파일인 경우, 압축을 해제해서 복사)
    **★ 일반적으로 COPY 사용을 권장**

    ##3 WORKDIR : cd 명령어와 동일, 명령어를 실행할 위치를 지정

    ##4 [ ]형식의 인자 (= JSON 배열 형식) : 쉘을 실행하지 않음을 의미

    RUN command 형식은 `/bin/sh -c command` 형식으로 실행

    ##5 EXPOSE : 이미지에서 노출할 port를 설정

    ##6 CMD : 컨테이너가 실행될 때 마다 실행할 명령어 (반드시 한번만 사용 가능)

3. **hello.html 파일을 생성**
`echo hello >> hello.html`
`ls hello.hml` (생성된 파일 확인)
`cat hello.html` (파일의 내용 확인)

4. **Dockerfile을 이용하여 이미지를 생성 및 확인**
`docker build -t myimage .
docker images`

    ![Day%209%20Dokcer_/Untitled%202.png](images/Day%209%20Dokcer_/Untitled%202.png)

5. **생성된 이미지로 컨테이너를 실행**
`docker run -d -P --name myserver myimage`

    `-P` : 호스트의 빈 port를 컨테이너에 EXPOSE된 포트로 매핑

6. **포트 확인** `docker container ls` , `docker port [container_name]`

    ![Day%209%20Dokcer_/Untitled%203.png](images/Day%209%20Dokcer_/Untitled%203.png)

7. **웹 서버 접속**

    ![Day%209%20Dokcer_/Untitled%204.png](images/Day%209%20Dokcer_/Untitled%204.png)

    - 컨테이너 중지 시 접속화면 (접속 불가)

        ![Day%209%20Dokcer_/Untitled%205.png](images/Day%209%20Dokcer_/Untitled%205.png)

### 컨테이너 중지/실행/삭제

---

- 컨테이너 실행 `docker container start [CONTAINER_ID]`

    ![Day%209%20Dokcer_/Untitled%206.png](images/Day%209%20Dokcer_/Untitled%206.png)

- 컨테이너 중지 `docker container stop [CONTAINER_ID]`

    ![Day%209%20Dokcer_/Untitled%207.png](images/Day%209%20Dokcer_/Untitled%207.png)

- 컨테이너 삭제 (중지 후, 삭제) `docker container rm CONTAINER_ID 또는 NAME`

    ![Day%209%20Dokcer_/Untitled%208.png](images/Day%209%20Dokcer_/Untitled%208.png)

- 실행중인 모든 컨테이너를 중지
`docker container stop $(docker container ls -q)`
- 모든 컨테이너를 삭제
`docker container rm -f $(docker container ls -aq)`

- 실행 중인 컨테이너와 동일한 이름으로 컨테이너를 실행 ⇒ **ERROR**

동일한 이름의 컨테이너가 존재하면, 컨테이너 상태(실행/중지)와 관계없이 새로운 컨테이너를 실행 시 오류가 발생함
**⇒ 강제적으로 컨테이너 삭제 후, 새로운 컨테이너를 실행해야 함**

- 강제적으로 컨테이너 삭제 후, 새로운 컨테이너를 실행
`docker container rm -f mywebserver ; docker run -d -P --name mywebserver myimage`

### (디렉토리, 파일 관련) 리눅스 명령어

---

- ./../bin/.a.sh
    1. 첫 번째 점 ⇒ 현재 디렉토리
    2. 두 번째 점 ⇒ 상위 디렉토리
    3. 세 번째 점 ⇒ 숨김(hidden) 파일
    4. 네 번째 점 ⇒ 파일 확장자 (.sh)

### (실습 2) 동일 이름의 컨테이너 삭제 후, 실행하는 쉘 스크립트 작성하기

---

- /webserver 하단에 쉘 스크립트 파일 생성 (run.sh)
- 다음과 같이 쉘 스크립트를 작성
    1. 명령어 형식 체크

        `if [ $# == 0 ]   ($# : 파라미터의 개수)
        then
        echo 명령어 형식이 잘못되었습니다.
        echo [사용법] ./run.sh container_name_or_id
        exit 1
        fi`

    2. 컨테이너 실행 전, 컨테이너 리스트를 출력
    `docker container ps -a`

    3. 동일 이름의 컨테이너를 조회
    `cid=$(docker container ps -a --filter="name=^/$1$" -q)`

    4. 동일 이름의 컨테이너가 존재하는 경우, 해당 컨테이너를 삭제 후 메시지를 출력 

        `if [ -n cid ]
        then
        docker container rm -f $cid
        echo $1 이름의 컨테이너\($cid\)를 삭제했습니다.
        fi`

    5. 컨테이너를 실행
    `docker container run --name $1 -d -P myimage`

    6. 컨테이너 실행 후, 컨테이너 리스트를 출력
    `docker container ps -a`

    7. 쉘 종료
    `exit 0`

- 생성된 쉘 스크립트 파일에 실행권한 부여
`chmod 755 run.sh`

- 파일 실행 `./ruh.sh mywebserver`

    ![Day%209%20Dokcer_/Untitled%209.png](images/Day%209%20Dokcer_/Untitled%209.png)

- 아래와 같은 형태로 기존의 컨테이너를 삭제하고 새롭게 컨테이너를 생성하는 스크립트를 작성하시오.

    [사용법] ./run.sh IMAGE_NAME CONTAINER_NAME

    1. CONTAINER_NAME 일치하는 컨테이너가 존재하는지 확인
    2. 존재하는 경우 해당 컨테이너를 삭제
    3. IMAGE_NAME 이미지를 이용해서 CONTAINER_NAME 이름의 컨테이너를 생성

### (실습 3) 이미지 파일에서의 환경변수

---

- `gedit Dockfile` 아래와 같이 작성

    FROM ubuntu
    
    ENV  workspace  /workspace
    
    RUN  mkdir $workspace
    
    WORKDIR $workspace
    
    RUN touch $workspace/mytouchfile

- Dockerfile 필드 (`envimage`로 명명)
`docker build -t envimage`
- `docker run -i -t -d envimage /bin/bash
docker attach [CONTAINER_ID 일부]` 컨테이너 안에서 작업
`echo $workspace workspace` 환경변수의 값 확인

![Day%209%20Dokcer_/Untitled%2010.png](images/Day%209%20Dokcer_/Untitled%2010.png)

![Day%209%20Dokcer_/Untitled%2011.png](images/Day%209%20Dokcer_/Untitled%2011.png)

-e : 컨테이너 실행 시 환경변수를 설정하는 옵션

### (실습 4) 호스트와 컨테이너 간 파일 복사

**호스트 ⇒ 컨테이너 로 파일 복사**
`docker container cp [HOST_FILE_PATH] [CONTAINER_ID_or_NAME]:[CONTAINER_FILE_PATH]`

**컨테이너 ⇒ 호스트**
`docker container cp [CONTAINER_ID_or_NAME]:[CONTAINER_FILE_PATH] [HOST_FILE_PATH]` 

- mywebserver 컨테이너 확인 `docker container ls`

- hello3.html 파일 생성 `echo hello3 >> hello3.html`
- 호스트의 hello3.html 파일을 mywebserver 컨테이너의 컨테이너 파일경로(/var/www/html/hello3.html)에 복사
`docker container cp ./hello3.html mywebserver:/var/www/html/hello3.html`
- 브라우저에 반영된 내용 확인

![Day%209%20Dokcer_/Untitled%2012.png](images/Day%209%20Dokcer_/Untitled%2012.png)

`docker stats` : 컨테이너의 실시간 자원 현황 (모니터링)