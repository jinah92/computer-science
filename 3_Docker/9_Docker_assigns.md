# (Day 9) Docker - MySQL 이미지를 이용한 컨테이너 생성

[mysql - Docker Hub](https://hub.docker.com/_/mysql)

⇒ 5.7 버전 링크에서 Dockerfile 확인 (github)

1. 작업 디렉토리(/wblog) 생성

    `root@server:~/webserver# cd ~
    root@server:~# mkdir wblog
    root@server:~# cd wblog/`

2. MySQL 이미지를 사용한 데이터베이스를 생성

    `root@server:~/wblog# docker run -d --name wordpressdb \`

    `-e MYSQL_ROOT_PASSWORD=passwd \
    -e MYSQL_DATABASE=wordpress \
    mysql:5.7`

3. 워드프레스를 이용한 웹 서버 컨테이너를 생성

    `root@server:~/wblog# docker run -d \`

    `-e WORDPRESS_DB_PASSWORD=passwd \
    --name wordpress \
    --link wordpressdb:mysql \`  ← 컨테이너의 별명(alias)로 접근할 수 있도록 설정
    `-p 80 \`   ← 호스트의 유효 port
    `wordpress`

4. 컨테이너 실행을 확인 (wordpress, wordpressdb) 

    ![Day%209%20Docker%20MySQL/Untitled.png](images/Day%209%20Docker%20MySQL/Untitled.png)

5. 호스트(Ubuntu)에서 [http://localhost:[workpress_port]](http://localhost:[workpress_port]) 로 접속

6. 실행 중인 컨테이너(mywebserver)를 이용하여 이미지를 생성
`docker commit -m "메시지" mywebserver [new_repository/new_image명:tag]`

    ![Day%209%20Docker%20MySQL/Untitled%201.png](images/Day%209%20Docker%20MySQL/Untitled%201.png)

7. 생성한 이미지로 컨테이너를 생성
`docker run --name [new_container_name] -d -P [new_repository/new_image:tag]`

8. 생성된 컨테이너 확인

![Day%209%20Docker%20MySQL/Untitled%202.png](images/Day%209%20Docker%20MySQL/Untitled%202.png)

모든 컨테이너와 이미지를 삭제
1. 모든 컨테이너를 중지 (stop)
   `docker container stop $(docker container ps -aq)`
2. 모든 컨테이너를 삭제 (rm -f)
   `docker contianer rm -f $(docker container ls -aq)`
3. 모든 이미지를 삭제 
   `docker image rm -f $(docker images -aq)`
4. Garbige를 제거 (`prune`)
   `docker contianer prune`
   `docker image prune`

### 과제 1

---

#1 작업디렉터리(lab) 생성

#2 아래 작업을 수행하는 Dockerfile을 생성

- ubuntu 최신 버전의 이미지를 베이스 이미지로 사용
- apache2 설치
- apache2를 백그라운드에서 실행

#3 docker build를 통해 이미지를 생성

- 이미지 이름 : myapach
- 이미지 태그 : latest

#4 호스트에서 생성한 index.html 파일을 컨테이너 내부 아파치 웹 루트에 복사

<html><body><h1>Hello, Docker</h1></body></html>

#5 호스트에서 웹 브라우저를 통해서 컨테이너로 접속

[http://localhost:??????/index.html](http://localhost:??????/index.html) 로 접속했을 때, Hello Dokcer가 출력되는 것을 확인

#6 현재 상태의 컨테이너의 이미지를 생성

- 이미지 이름 : myapache
- 이미지 태그 : 1.0

#7 #6에서 생성한 이미지를 자신의 도커 허브에 반영

#8 #7에서 반영한 이미지를 다른 사람의 자리에서 가져와서 실행 후, 브라우저를 통해 확인

- **결과 화면**

![Day%209%20Docker%20MySQL/Untitled%203.png](images/Day%209%20Docker%20MySQL/Untitled%203.png)

![Day%209%20Docker%20MySQL/Untitled%204.png](images/Day%209%20Docker%20MySQL/Untitled%204.png)

### 과제2

---

LAB2

아래 조건을 만족하는 Dockerfile을 만들고 이미지를 빌드

- ubuntu 최신 버전을 베이스 이미지로 지정
- [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/) 내용을 참조하여 도커를 설치
- 이미지 이름을 dind로 태그명을 latest로 생성

생성한 이미지로 컨테이너를 생성

- 사용자 입력을 받을 수 있도록 하고, 백그라운드에서 실행될 수 있도록 한다.
- 컨테이너 실행시 /bin/bash 쉘이 실행되도록 한다.

실행된 컨테이너에 접속하여 docker --version 실행 결과를 확인

[실행예]

root@014130d892cf:/# docker --version

Docker version 19.03.5, build 633a0ea838

- **결과 화면**

![Day%209%20Docker%20MySQL/Untitled%205.png](images/Day%209%20Docker%20MySQL/Untitled%205.png)