# (Day 8) - Docker 설치하기

Created: Dec 23, 2019 11:34 AM
Tags: Docker, Docker Container, 쉘 프로그래밍
Updated: Dec 23, 2019 9:54 PM

## 윈도우용 Docker 사용하기

---

1. Windows 기능 켜기/끄기 ⇒ **Hyper-V 활성화**
2. Windows 10 Pro 이하 ⇒ **① 도커 + ② 도커 툴 박스 를 모두 설치**

## 포트 포워딩

---

IP를 나눠쓰는 환경(NAT)에서 외부 네트워크에서 내부 네트워크로 접속하는 것을 도와줌

- Virtual Network Editor
    1. Change Settings (관리자 권한으로 실행)

        ![Day%208%20Docker/Untitled.png](images/Day%208%20Docker/Untitled.png)

    2. Host Port : 내부 네트워크에서 외부 네트워크를 식별하는 포트 
    Virtual machine IP 주소 : 가상머신(내부 네트워크의 호스트) IP 주소
    Virtual machine Port : 가상머신(내부 네트워크의 호스트) 포트 (apache 웹서버)

        ![Day%208%20Docker/Untitled%201.png](images/Day%208%20Docker/Untitled%201.png)

    3. 외부 네트워크에서 접속 시도 (192.168.111.129:1234)

        ![Day%208%20Docker/Untitled%202.png](images/Day%208%20Docker/Untitled%202.png)

## 1. Docker 설치하기

---

1. **Docker Repository(도커 저장소) 추가**

    `gedit /etc/apt/sources.list`

    하단에 입력:  deb [https://apt.dockerproject.org/repo](https://apt.dockerproject.org/repo) ubuntu-xenial main

    ![Day%208%20Docker/Untitled%203.png](images/Day%208%20Docker/Untitled%203.png)

2. HTTPS 통신을 위한 패키지 및 공개키 설치
`apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common`

`apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D`

3. linux-image extra 및 docker-engine 패키지 설치
`apt-get install linux-image-extra-$(uname -r)`
`apt-get install docker-engine`

    **$**(uname -r) ← 환경에 맞는 변수를 이용하여 명령어를 실행

  4. Docker 버전 확인  `docker version`

![Day%208%20Docker/Untitled%204.png](images/Day%208%20Docker/Untitled%204.png)

## 2. Docker 이미지 생성/실행/종료

---

1. **작업 디렉토리(docker) 및 main.go 파일 생성**
`mkdir docker`
`cd docker
gedit main.go`

        // 8080 포트로 대기하는 웹 서버에 /로 요청이 들어오면
        // 응답메시지 "Hello Docker"를 반환
        package main
        
        import (
        	"fmt"
        	"log"
        	"net/http"
        )
        
        func main() {
        	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        		log.Println("received request")
        		fmt.Fprintf(w, "Hello Docker!!")
        	})
        
        	log.Println("start server")
        	server := &http.Server{Addr: ":8080"}
        	if err := server.ListenAndServe(); err != nil {
        		log.Println(err)
        	}
        }

2. **main.go 파일 실행**
    - go 패키지 설치 `apt-get install golang-go`
    - main.go 실행   `go run main.go`

    ![Day%208%20Docker/Untitled%205.png](images/Day%208%20Docker/Untitled%205.png)

    - (새 터미널) `curl [http://localhost:8080/](http://localhost:8080/)`
    - (브라우저)

        ![Day%208%20Docker/Untitled%206.png](images/Day%208%20Docker/Untitled%206.png)

3. **Dockerfile 생성**  `gedit Dockerfile`

        FROM golang:1.9   // 베이스 이미지(Docker Hub의 공식 이미지)를 가져옴
        									// (저장소 이름 생략)
        
        RUN mkdir /echo   // 컨테이너 내부에 /echo 디렉토리를 생성
        
        COPY main.go /echo // /echo 디렉토리에서 main.go파일 실행
        
        CMD [ "go", "run", "/echo/main.go" ]

    - Docker 이미지 관련 옵션 확인 `--help` 활용
    `docker image --help`

4. **Docker 이미지 빌드 (Pull)**
`docker image build -t example/echo:latest .`
5. **Docker 이미지 확인** `docker iamges`  or `docker image ls`

    ![Day%208%20Docker/Untitled%207.png](images/Day%208%20Docker/Untitled%207.png)

6. **Docker 컨테이너 실행**  `docker container`

    ![Day%208%20Docker/Untitled%208.png](images/Day%208%20Docker/Untitled%208.png)

    백그라운드에서 실행 `-d`
    `docker container run -p 9000:8080 -d example/echo:latest`

    ![Day%208%20Docker/Untitled%209.png](images/Day%208%20Docker/Untitled%209.png)

    호스트와 컨테이너 간 입력 공유 `-it`
    `docker container run -p 9000:8080 -it example/echo:latest`
    (입력 가능, 강제 종료 가능)

    ![Day%208%20Docker/Untitled%2010.png](images/Day%208%20Docker/Untitled%2010.png)

    백그라운드에서 실행 + 입력 (제어권을 가지고 컨테이너에 작업) `-itd`
    `docker container run -p 9000:8080 -itd example/echo:latest`

    ![Day%208%20Docker/Untitled%2011.png](images/Day%208%20Docker/Untitled%2011.png)

    쉘 프로그래밍 (뒤에 쉘 파일 삽입 `/bin/bash` )
    `docker container run -p 9000:8080 -itd example/echo:latest /bin/bash`

    ![Day%208%20Docker/Untitled%2012.png](images/Day%208%20Docker/Untitled%2012.png)

    이름으로 제어/실행 `--name CONTAINER_NAME`
    `docker container run -p 9003:8080 -itd --name CONTAINER_NAME example/echo:latest /bin/bash`

7. **백그라운드에서 실행되는 컨테이너 접속**
`docker attach CONTAINER_ID`

8. **현재 실행 중인 Docker 컨테이너 상태 확인**
`docker container ps
docker container ps -a`
`docker container ls
docker container ls -l`

9. **실행 중인 Docker 컨테이너에서 나오는 방법**
    - (입력을 받을 수 없는 경우) 다른 터미널에서 `docker container stop CONTAINER_ID`
    - (입력을 받을 수 있는 경우) `Ctrl+C` or `Ctrl+PQ`
    - (쉘 제공) `eixt` or `Ctrl+PQ`
10. **도커 컨테이너 실행/중지**
    - (실행) `docker container start CONTAINER_ID(or name)`
    - (중지) `docker container stop CONTAINER_ID(or name)`
    - (실행 중인 모든 컨테이너 중지) `docker container stop $(docker container ls -q)`

        ![Day%208%20Docker/Untitled%2013.png](images/Day%208%20Docker/Untitled%2013.png)

11. **동일 조상의 컨테이너를 listing** `"ancestor = [CONTAINER_FILE]"`

    ![Day%208%20Docker/Untitled%2014.png](images/Day%208%20Docker/Untitled%2014.png)

## Docker Hub

---

### 이미지 태그(tag)

- `DOCKERHUB_ID(=Repository)/IMAGE_NAME:TAG_NAME`
- 동일한 도커 이미지에 태그(1.0)를 설정하여 생성

    ![Day%208%20Docker/Untitled%2015.png](images/Day%208%20Docker/Untitled%2015.png)

### 도커 이미지를 도커 허브에 등록

- 이미지명을 `DOCKERHUB_ID/IMAGE_NAME:TAG_NAME` 형식을 준수해야 함
- `docker login` 명령어로, docker hub에 로그인
- `docker image push` 명령어로, 이미지를 등록

    ![Day%208%20Docker/Untitled%2016.png](images/Day%208%20Docker/Untitled%2016.png)

    ![Day%208%20Docker/Untitled%2017.png](images/Day%208%20Docker/Untitled%2017.png)

- `docker search DOCKERHUB_ID/IMAGE_NAME` 로 확인