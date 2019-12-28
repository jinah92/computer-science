# (Day 10) Docker Container 데이터 보존 (+docker exec)

### 1. 외부에서 컨테이너 내부의 명령어 실행 : docker exec

---

- Unbuntu (14.04 버전)의 이미지를 빌드
`docker run -d --name echo ubuntu:14.04 /bin/bash -c "while true; do echo Hello Docker; sleep 1; done"`
- 컨테이너 외부에서 명령어 실행
`docker exec -it echo /bin/bash`

docker export <옵션> <컨테이너 이름, ID> <명령> <매개 변수>

-d, --detach=false: 명령을 백그라운드로 실행
-i, --interactive=false: 표준 입력(stdin)을 활성화하며 컨테이너와 연결(attach)되어 있지 않더라도 표준 입력을 유지
-t, --tty=false: TTY 모드(pseudo-TTY)를 사용. Bash를 사용하려면 이 옵션을 설정해야 합니다. 이 옵션을 설정하지 않으면 명령을 입력할 수는 있지만 셸이 표시되지 않습니다.

주의 : docker exec 명령을 할때 옵션으로 `-it` 라고 덧붙여 주어야 한다. 이는 STDIN 표준 입출력을 열고 가상 tty (pseudo-TTY) 를 통해 접속하겠다는 의미이다.

![Day%2010%20Docker%20Container%20docker%20exec/Untitled.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled.png)

- docker exec로 사용가능한 패키지, 버전 리스트를 업데이트 & 웹 서버(apache2) 패키지를 설치
`docker exec echo apt-get update 
docker exec echo apt-get install apache2 -y`

### 2. 도커 컨테이너, 이미지 삭제 (prune)

---

- 컨테이너 삭제
    1. 모든 컨테이너의 가동을 중지
    `docker container stop $(docker container ls -aq)`
    2. 모든 컨테이너를 삭제
    `docker container rm $(docker container ls -aq)`
    3. 한번에 모든 컨테이너를 삭제
    `docker container prune`

- 이미지 삭제
    - `docker image rm -f $(docker images -aq)`
    - `docker image prune`

### 컨테이너의 데이터를 영속적으로 사용하는 방법 3가지

- 도커 이미지로 컨테이너를 생성하면, 이미지는 '읽기 전용'이 되고 컨테이너의 변경 부분만 별도로 저장해서 컨테이너의 정보를 보존한다. 컨테이너가 삭제되면 컨테이너에 저장된 데이터도 함께 삭제된다. ⇒ 이런 문제를 해결하기 위해 데이터를 영속적으로 저장하는 방법이 몆가지 있다.
- **호스트 볼륨 / 볼륨 컨테이너 / 도커 볼륨**

### 방법 1. 호스트 볼륨 공유(Docker Volume)
컨테이너 데이터를 호스트 디스크에 저장

---

- 도커 볼륨
    1. 도커 컨테이너에 의해 생성되고 도커 컨테이너에 의해 사용되는 데이터를 유지하는 기본 메커니즘
    2. 볼륨은 도커에 의해 완벽하게 관리되고, bind mount에 비해 몇 가지 장점이 있다.
        - 볼륨을 백업 또는 마이그레이션  하는 것이 쉬움
        - CLI 또는 API를 사용하여 관리가 가능
        - 리눅스 및 윈도우 컨테이너에서 작동
        - 여러 컨테이너 간에 안전하게 공유 가능
        - 볼륨 드라이버를 사용하면 원격 호스트 또는 클라우드 공급자에게 볼륨을 저장하고, 볼륨 내용을 암호화하거나 다른 기능을 추가할 수 있음
        - 새 볼륨은 컨테이너에 의해 미리 채워진 내용을 가질 수 있음

        ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%201.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%201.png)

- **호스트 볼륨을 공유하는 방법**
    - 컨테이너를 생성할 때 `-v` 옵션을 사용
    - `-v [호스트 디렉터리 또는 파일]:[컨테이너 디렉터리 또는 파일]`

    1. 마운트하려는 볼륨이 호스트에 존재하지 않으면, 호스트에 해당 디렉토리가 생성되며 컨테이너와 공유됨
    2. 호스트와 컨테이너에 모두 디렉토리가 존재하면, 컨테이너의 파일이 삭제되고 호스트의 파일이 공유됨

### 실습

---

- MySQL 이미지를 이용하여 데이터베이스 컨테이너를 생성

    root@server:~/docker# docker run -d \

    > --name wordpressdb_hostvolume \

    > -e MYSQL_ROOT_PASSWORD=password \

    > -e MYSQL_DATABASE=wordpress \

    > **-v /home/wordpress_db:/var/lib/mysql** \

    > mysql:5.7

    **-v /home/wordpress_db:/var/lib/mysql
     ⇒ 호스트의 /home/wordpress_db 디렉토리를
     ⇒ 컨테이너의 /var/lib/mysql 디렉토리로 공유
     (mysql db의 데이터를 저장하는 기본 디렉토리)**

- 워드프레스 이미지를 이용하여 웹 서버 컨테이너를 생성

    root@server:~/docker# docker run -d --name wordpress_hostvolume \

    > -e WORDPRESS_DB_PASSWORD=password \

    > --link wordpressdb_hostvolume:mysql \

    > -p 80 \

    > wordpress

- 호스트 볼륨 공유를 확인

    `ls [호스트의 디렉토리]`

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%202.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%202.png)

    `docker exec [컨테이너 id] ls [컨테이너의 디렉토리]`

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%203.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%203.png)

- 컨테이너 삭제 후, 볼륨 데이터가 유지되는 지를 확인

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%204.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%204.png)

- 파일 단위의 공유 & `-v` 옵션을 여러 개 사용 가능
    1. 호스트에 hello1.txt와 hello2.txt 파일을 생성
    2. 위 2 파일을 컨테이너와 공유

        root@server:~/docker# docker run -it \

        --name volume_test2 \

        -v /root/docker/hello1.txt:/hello1.txt \

        -v /root/docker/hello2.txt:/hello2.txt \

        ubuntu:14.04

    3. 생성된 컨테이너 안에서 볼륨을 확인

        ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%205.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%205.png)

    4. (호스트에서) hello1.txt 내용을 변경 후, 호스트와 컨테이너에 반영되는지를 확인
    5. (컨테이너에서) hello2.txt 내용을 변경 후, 호스트와 컨테이너에 반영되는지를 확인

- 컨테이너에 존재하는 디렉토리를 호스트 볼륨으로 공유하는 경우
- alicek105/volume_test 이미지로 컨테이너를 실행

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%206.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%206.png)

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%207.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%207.png)

    - 존재하는 호스트 디렉토리 (/home/wordpress_db)와 공유

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%208.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%208.png)

### 방법 2. 볼륨 컨테이너

---

- 컨테이너를 실행할 때 `--volumes-from` 옵션을 사용
⇒  `-v` (`--volume`) **옵션을 적용한 컨테이너 (= 볼륨 컨테이너)** 의 볼륨 디렉토리를 공유
⇒ 하단에서의 볼륨 컨테이너 : volume_overide

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%209.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%209.png)

### 방법 3. 도커 볼륨

---

- 도커 자체에서 제공하는 볼륨 기능을 활용하여 데이터를 보존
- 명령어 : `docker volume`

- 도커 볼륨 생성 `docker volume create --name [도커볼륨이름]`
- 도커 볼륨 조회 `docker volume ls`

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%2010.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%2010.png)

- 생성한 볼륨을 사용하는 컨테이너 생성  `-v [볼륨 이름]:[컨테이너 디렉토리]`

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%2011.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%2011.png)

- 동일한 볼륨을 사용하는 컨테이너(myvolume_2)를 생성해서, 파일 공유가 되는지를 확인
- 볼륨 myvolume과 생성하는 컨테이너의 디렉토리 /temp/를 공유

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%2012.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%2012.png)

- 볼륨 정보 조회 `docker inspect`

    ![Day%2010%20Docker%20Container%20docker%20exec/Untitled%2013.png](images/Day%2010%20Docker%20Container%20docker%20exec/Untitled%2013.png)