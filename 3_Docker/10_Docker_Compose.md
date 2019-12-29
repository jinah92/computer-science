# (Day 10) Docker Compose

Created: Dec 28, 2019 8:41 PM
Tags: Docker, Docker Compose, Docker Container
Updated: Dec 29, 2019 1:45 PM

- 컨테이너 여럿을 띄우는 도커 어플리케이션을 정의 및 실행하는 도구
- `docker-compose.yml` 파일에 도커 컨테이너 실행에 필요한 옵션을 정의할 수 있고, 컨테이너 간의 의존성을 관리할 수 있음

[Docker vs Docker Compose](https://www.notion.so/a409a91d723449bfb545b282ec1d9fc9)

![Day%2010%20Docker%20Compose/Untitled.png](Day%2010%20Docker%20Compose/Untitled.png)

### 실습

---

- **도커 설치**
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
    1. `curl -L "[https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$](https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$)(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
    2. 실행권한 (x) 부여
    `chmod +x /usr/local/bin/docker-compose`
    3. 설치여부 확인 (버전 확인)
    `docker-compose —version`

- **작업할 디렉토리 (/compose) 생성 후, 이동**
    1. 홈디렉토리로 이동 `cd`
    2. 작업 디렉토리 생성 `mkdir compose`
    3. 작업 디렉토리 이동 `cd compose`

- **docker compose를 이용한 이미지 생성 및 실행**
    1. docker-compose.yml 파일 생성

            version: "3"
            services:
                echo: 
                    image: myanjini/echo:latest
                    ports: 
                        - 9090:8080

    2. 생성한 이미지로 컨테이너를 생성 `docker-compose up`

        ![Day%2010%20Docker%20Compose/Untitled%201.png](Day%2010%20Docker%20Compose/Untitled%201.png)

        -p 옵션 : compose project name 지정
        `docker-compose -p [COMPOSE PROJECT NAME] up` 
        `docker-compose -p [COMPOSE PROJECT NAME] down` 

    3. (다른 터미널에서 작업)

        (1) **실행한 컨테이너 확인**

        ![Day%2010%20Docker%20Compose/Untitled%202.png](Day%2010%20Docker%20Compose/Untitled%202.png)

        (2) 실행 중인 컨테이너(웹 서버)에 접근 / 서버 응답 (Hello Dokcer!!)

        ![Day%2010%20Docker%20Compose/Untitled%203.png](Day%2010%20Docker%20Compose/Untitled%203.png)

        ![Day%2010%20Docker%20Compose/Untitled%204.png](Day%2010%20Docker%20Compose/Untitled%204.png)

        (3) 컨테이너(웹 서버) 중지
        `docker-compose down`

        ![Day%2010%20Docker%20Compose/Untitled%205.png](Day%2010%20Docker%20Compose/Untitled%205.png)