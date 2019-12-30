# (Day 1) Docker Swarm

Created: Dec 30, 2019 9:17 AM
Tags: Docker, Swarm, node

## 개요

---

- 도커에서 제공해주는 분산 환경에서의 코디네이터
- **도커 스웜, 도커 스웜 모드**
    1. 여러 대의 도커 서버를 하나의 클러스터로 만들어 컨테이너를 생성하는 기능
    2. 도커 스웝
        - 도커 1.6 버전 이후부터 사용
        - 에이전트 컨테이너가 필요하며, 분산 코디네이터가 외부에 존재해야 함
        - 여러 대의 도커 서버를 하나의 지점부터 사용하도록 **단일 접근점(Access Point)**을 제공
    3. 도커 스웜 모드
        - 도커 1.12 버전 이후부터 사용
        - 에이전트가 도커 자체에 내장 (분산 코디네이터를 외부에 설치할 필요 없음)
        - **클러스터링 기능**에 초점

    도커 스웜과 도커 스웜모드는 **최소 3개 이상의 우분투 서버**가 필요

- 일반적인 클러스터 구성
    1. **분산 코디네이터** : 각종 정보를 저장하고 동기화 ⇒ 클러스터에 영입할 새로운 서버의 발견, 클러스터의 각종 설정을 저장, 데이터 동기화 등에 사용
    2. **매니저** : 클러스터 내의 서버를 관리 및 제어
    3. **에이전트** : 각 서버를 제어

- 도커 스웜 모드 ⇒ 매니저 노드와 워커 노드로 구성
    1. **매니저 노드** : 워커 노드를 관리하기 위한 도커 노드
    2. **워커 노드** : 실제 컨테이너가 생성되고 관리되는 도커 노드
    3. 매니저 노드에도 컨테이너가 생성될 수 있음 = 매니저 노드는 기본적으로 워커 노드 역할을 포함
    4. 매니저 노드는 반드시 1개 이상 존재해야 함 (운영환경에서는 다중화하는 것을 권장)
    5. 매니저 노드의 절반 이상에 장애가 발생하는 경우, 복구를 위해 클러스터 운영을 중지하므로 매니저 노드는 홀수개로 구성하는 것이 효율적

### 실습

---

1. **우분투 서버 3개를 생성 (도커가 설치되어 있어야 함)**

    도커 스웜 지원여부 확인
    `docker —version` ← 버전이 1.12 이상
    `docker info | grep Swarm`

2. **각 서버의 이름 설정 (swarm-manager, swarm-worker1, swarm-worker2)**

    ![Day%201%20Docker%20Swarm/Untitled.png](images/Day%201%20Docker%20Swarm/Untitled.png)

3. **가상머신별로 호스트명 변경 ⇒ 리부팅 후 확인**

    ![Day%201%20Docker%20Swarm/Untitled%201.png](images/Day%201%20Docker%20Swarm/Untitled%201.png)

    ![Day%201%20Docker%20Swarm/Untitled%202.png](images/Day%201%20Docker%20Swarm/Untitled%202.png)

4. **가상머신별 ip 확인**

5. **매니저 역할의 서버에서 스웜 클러스를 시작**

    root@**swarm-manager**:~# `docker swarm init --advertise-addr 192.168.111.100(매니저 서버의 ip주소)`

    Swarm initialized: current node (wjbpqvzbsmjheruavqo8h9gij) is now a manager.

    To add a worker to this swarm, run the following command:

    docker swarm join \

    --token SWMTKN-1-1ijy2o5balgzurh7dd68efv7304iofq3gzn6ijhqx3atxwyont-4f3n7u2beok42q25amqzc0f3e \

    `192.168.111.100:2377 ⇐ 새로운 워커 노드를 클러스터에 추가할 때 사용하는 비밀키(토큰)`

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

6. **워커 노드를 추가**

root@**swarm-worker1**:~# `docker swarm join \`

    `> --token SWMTKN-1-1ijy2o5balgzurh7dd68efv7304iofq3gzn6ijhqx3atxwyont-4f3n7u2beok42q25amqzc0f3e \`
    
    `> 192.168.111.100:2377`
    
    This node joined a swarm as a worker.
    
    root@**swarm-worker2**:~# `docker swarm join \`
    
    `> --token SWMTKN-1-1ijy2o5balgzurh7dd68efv7304iofq3gzn6ijhqx3atxwyont-4f3n7u2beok42q25amqzc0f3e \`
    
    `> 192.168.111.100:2377`
    
    This node joined a swarm as a worker.

7. **도커 서버가 정상적으로 스웜 클러스트에 추가되었는지 확인**
root@swarm-manager:~# `docker node ls`
od3yoj1n0y2aogavgzkp7el8e * swarm-manager Ready Active Leader
oo2vx2mpu6qbwvicu1gq85jsb swarm-worker2 Ready Active
uj2b9i6jfvryv5jlsz1hy0fr2 swarm-worker1 Ready Active

    *  ← 현재 명령어가 실행되는 위치

8. **토큰 확인 및 변경** 

**(확인)**

    root@swarm-manager:~# `docker swarm join-token manager`
    To add a manager to this swarm, run the following command:
    
        docker swarm join \
        --token SWMTKN-1-5cj670wvska9w0e8vfvghhy7wm3s8v1w174p7u1cq3qzf8s45o-0m3x7butwvtf37wq7z2hqu1x1 \
        192.168.10.129:2377
    
    root@swarm-manager:~# `docker swarm join-token worker`
    To add a worker to this swarm, run the following command:
    
        docker swarm join \
        --token SWMTKN-1-5cj670wvska9w0e8vfvghhy7wm3s8v1w174p7u1cq3qzf8s45o-d5okidnx2cggayijwc5zegcpp \
        192.168.10.129:2377
    
    **(변경)**
    root@swarm-manager:~# `docker swarm join-token --rotate manager`
    Successfully rotated manager join token.
    
    To add a manager to this swarm, run the following command:
    
        docker swarm join \\
        --token SWMTKN-1-5cj670wvska9w0e8vfvghhy7wm3s8v1w174p7u1cq3qzf8s45o-dmw0z88jmqdroxwody4zi1xj6 \\
        192.168.10.129:2377

9. **도커 노드 삭제**

    **root@swarm-worker1**:~# `docker swarm leave`  (클러스터에서 빠지기)
    Node left the swarm.

    ---

    **root@swarm-manager**:~# `docker node ls`       (빠진 클러스터를 확인)
    ID HOSTNAME STATUS AVAILABILITY MANAGER STATUS
    od3yoj1n0y2aogavgzkp7el8e * swarm-manager Ready Active Leader
    oo2vx2mpu6qbwvicu1gq85jsb swarm-worker2 Ready Active
    uj2b9i6jfvryv5jlsz1hy0fr2 swarm-worker1 Down Active

    ---

    **root@swarm-manager**:~# `docker node rm swarm-worker1`  (worker1을 클러스트에서 삭제)
    swarm-worker1

    root@swarm-manager:~# `docker node ls`                           (worker1이 삭제된 것을 확인)
    ID HOSTNAME STATUS AVAILABILITY MANAGER STATUS
    od3yoj1n0y2aogavgzkp7el8e * swarm-manager Ready Active Leader
    oo2vx2mpu6qbwvicu1gq85jsb swarm-worker2 Ready Active

10. **노드의 역할을 변경
`docker node promote`  ← 워커 노드를 '매니저 노드'로 변경
`docker node demote`   ← 매니저 노드를 '워커 노드'로 변경**

    - **새로운 클러스터 구성**
    root@swarm-manager:~# `docker swarm init`  (미입력시 로컬주소로 클러스터 생성)
    Swarm initialized: current node (n1ev6r30coiprsygh26uge2cn) is now a manager.

        To add a worker to this swarm, run the following command:

            docker swarm join \\
            --token SWMTKN-1-07vw2s9o3y38nywk1p1etujfoxa115pukpovzdt0baagt8fsd4-26vnz9sjgagitb2p2w43qlaif \\
            192.168.10.129:2377

        To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

    - **워커 노드를 추가**
    root@swarm-worker1:~# `docker swarm join \`

        `--token SWMTKN-1-07vw2s9o3y38nywk1p1etujfoxa115pukpovzdt0baagt8fsd4-26vnz9sjgagitb2p2w43qlaif \\
        192.168.10.129:2377`

        This node joined a swarm as a worker.

    - 매니저 노드를 워커 노드로 변경

        root@swarm-manager:~# `docker node demote swarm-manager`
        Error response from daemon: rpc error: code = 9 desc = attempting to demote the last manager of the swarm
        ⇒ 매니저 노드가 1개인 경우, demote 불가

        ---

        root@swarm-manager:~# `docker node demote swarm-manager`

        Manager swarm-manager demoted in the swarm.

        root@swarm-manager:~# `docker node ls`

        Error response from daemon: This node is not a swarm manager. Worker nodes can't be used to view or modify cluster state. Please run this command on a manager node or promote the current node to a manager.
        ⇒ 워크 노드로 변경되어, 노드 조회 불가

        ---

        root@swarm-worker1:~# docker node ls

        ID HOSTNAME STATUS AVAILABILITY MANAGER STATUS

        3x7nnbkxzhvlbr9mqtolrdnuy swarm-manager Ready Active

        70wn2r7r76ixjf62dg1n0ae5b * swarm-worker1 Ready Active Leader

        bibzwy94p2gq61fvybj79hv70 swarm-worker2 Ready Active

    - **워커 노드(worker1) 를 매니저 노드로 변경**
    root@swarm-manager:~# `docker node promote swarm-worker1`

        Node swarm-worker1 promoted to a manager in the swarm.

        ![Day%201%20Docker%20Swarm/Untitled%203.png](images/Day%201%20Docker%20Swarm/Untitled%203.png)

11. **서비스**

서비스 = **같은 이미지로 생성된 컨테이너의 집합
※ 서비스 제어는 '매니저 노드'에서만 가능**

- **서비스 생성**
root@swarm-manager:~# `docker service create \`

    `ubuntu:14.04 \
    /bin/bash -c "while true; do echo Hello Docker; sleep 1; done"`
    ivbjkiclkuon67n8o8gj4dcim
    Since --detach=false was not specified, tasks will be created in the background.
    In a future release, --detach=false will become the default.  ← 도커 버전에 따라 출력내용 상이

- **서비스 확인**
root@swarm-manager:~# `docker service ls`
ID NAME MODE REPLICAS IMAGE PORTS
ivbjkiclkuon musing_sinoussi replicated 1/1 ubuntu:14.04

  ---

    (서비스 상세확인) `docker service ps [SERVICE_NAME]`

    ![Day%201%20Docker%20Swarm/Untitled%204.png](images/Day%201%20Docker%20Swarm/Untitled%204.png)

    ⇒ worker1에 서비스(musing_sinoussi) 생성

  ---

    ![Day%201%20Docker%20Swarm/Untitled%205.png](images/Day%201%20Docker%20Swarm/Untitled%205.png)

- 서비스 삭제 (서비스 상태와 관계 없이 삭제 가능)
`docker service rm [SERVICE_NAME]`

- nginx 웹 서비스를 생성

    root@swarm-manager:~# `docker service create \`

    > `--name myweb \`

    > `--replicas 2 \`

    > `-p 8080:80 \`

    > `nginx`

    05g5mjdi54r9sslb4gqnszea5

    Since --detach=false was not specified, tasks will be created in the background.

    In a future release, --detach=false will become the default.

    ---

    root@swarm-manager:~# `docker service ls`

    ![Day%201%20Docker%20Swarm/Untitled%206.png](images/Day%201%20Docker%20Swarm/Untitled%206.png)

    ---

    #2 nginx로 접속

    http://SWARM_MANAGER_IP:8080/

    http://SWARM_WORKER1_IP:8080/

    http://SWARM_WORKER2_IP:8080/

    ⇒ **컨테이너 실행 여부와 관계 없이** 접속을 확인

    → **각 호스트의 어느 노드로 접근하든 실행 중인 컨테이너로 접속이 가능**

    #3 리플리카 개수를 변경

    root@swarm-manager:~# `docker service scale myweb=4`
    myweb scaled to 4  ⇒ 리플리카 개수가 4개로 변경됨

    ---

    생성된 service의 상세목록 확인 `docker service ps [SERVICE NAME]`

    ![Day%201%20Docker%20Swarm/Untitled%207.png](images/Day%201%20Docker%20Swarm/Untitled%207.png)

**1. 복제 모드 (default)** : 정의된 리플리카 개수만큼 컨테이너가 생성
**2. 글로벌 모드** : 모든 노드에 컨테이너를 생성 (`docker service create —mode global` 옵션)

- **서비스 장애 복구**

    #1 매니저 노드에서 노드를 확인
    root@swarm-manager:~# `docker node ls`
    (1개의 매니저 노드, 2개의 워커 노드가 실행 중)

    ![Day%201%20Docker%20Swarm/Untitled%208.png](images/Day%201%20Docker%20Swarm/Untitled%208.png)

    root@swarm-manager:~# `docker service ls`
    (4개의 task가 동작 중)

    ![Day%201%20Docker%20Swarm/Untitled%209.png](images/Day%201%20Docker%20Swarm/Untitled%209.png)

    #2 매니저 노드에서 실행 중인 컨테이너를 삭제

    #3 스웜 노드(swarm-worker1)에 장애가 발생한 경우

    root@swarm-worker1:~# `service docker stop`

    ![Day%201%20Docker%20Swarm/Untitled%2010.png](images/Day%201%20Docker%20Swarm/Untitled%2010.png)

    root@swarm-manager:~# `docker service ps myweb`

    ![Day%201%20Docker%20Swarm/Untitled%2011.png](images/Day%201%20Docker%20Swarm/Untitled%2011.png)

- 서비스 롤링 업데이트
#1 기존 모든 서비스를 삭제
`docker service rm [SERVICE_NAME]`

#2 새로운 서비스 생성
`docker service create —name [SERVICE_NAME] —replicas [num] [image]`

    ![Day%201%20Docker%20Swarm/Untitled%2012.png](images/Day%201%20Docker%20Swarm/Untitled%2012.png)

#3 이미지 변경 (nginx:1.10 ⇒ nginx:1.11)
`docker service update —image nginx:1.11 [SERVICE_NAME]`

(변경사항 확인)

![Day%201%20Docker%20Swarm/Untitled%2013.png](images/Day%201%20Docker%20Swarm/Untitled%2013.png)

#4 업데이트 조건과 합께 서비스를 생성
# `docker service create \`
`—-replicas 4 \`

`—-name myweb3 \`

`—-update-delay 10s \`
`—-update-failure-action continue \`

`nginx:1.10`

#5 서비스 롤백
`docker service rollback [SERVICE_NAME]` ← 최신 도커 버전에서 사용 가능
`docker service update —rollback [SERVICE_NAME]` ← 낮은 버전

![Day%201%20Docker%20Swarm/Untitled%2014.png](images/Day%201%20Docker%20Swarm/Untitled%2014.png)