# HyperLedger

## Linux Ubuntu 18.04

### IBM cloud

1. IBM cloud에서 HW, OS 등을 설정하여 공인 IP 발급(유료)

2. putty를 이용하여 ssh포트(22)로 접속
3. IBM cloud에서 부여받은 root계정 password를 입력해서 리눅스에 로그인



### basic-network

1. apt update

2. apt install -y docker docker-compose golang-go npm

3. npm i n -g

4. go env

   ```
   ...
   GOPATH="/root/go"
   GOROOT="/usr/lib/go-1.10"
   ...
   
   //이런식으로 GOPATH와 GOROOT를 확인후 환경변수에 등록해준다.
   ```

5. vi ~/.bashrc

   ```
   export GOPATH="/root/go"
   export GOROOT="/usr/lib/go-1.10"
   
   //위에서 확인한 GOPATH와 GOROOT를 등록
   ```

6. echo \$GOROOT && echo \$GOPATH //로 적용 되었는지 확인

7. ~/ mkdir HLF

8. cd HLF //여기에 하이퍼렛저 패브릭을 설치할 것임

9. curl -sSL http://bit.ly/2ysbOFE|bash -s 1.4.3 // 도커 이미지와 하이퍼렛저 패브릭 v1.4.3 관련 파일들이 설치됨

10. docker images 로 이미지들 확인

    basic-network는 ca, oderer, 피어, couchdb, cli로 구성할 것이며 각각의 docker-compose.yaml에 의해서 구성되는 컨테이너 이름은 ca.example.com, orderer.example.com, peer0.org1.example.com, couchdb, cli이다.

    \* couchdb와 ca는 실제 하이퍼렛저 네트워크에 존재하지 않는다. couchdb는 피어들마다 로컬로 가지고 있고 ca는 인증기관이다.

11. cd basic-network // curl을 성공하면 생성되는 폴더이다

12. gedit start.sh //cli를 추가적으로 실행하기 위해 셸 스크립트를 수정해준다.

    ```sh
    #!/bin/bash
    #
    # Copyright IBM Corp All Rights Reserved
    #
    # SPDX-License-Identifier: Apache-2.0
    #
    # Exit on first error, print all commands.
    set -ev
    
    # don't rewrite paths for Windows Git Bash users
    export MSYS_NO_PATHCONV=1
    
    docker-compose -f docker-compose.yml down
    
    docker-compose -f docker-compose.yml up -d ca.example.com orderer.example.com peer0.org1.example.com couchdb cli #여기에 cli 추가
    docker ps -a
    
    # wait for Hyperledger Fabric to start
    # incase of errors when running later commands, issue export FABRIC_START_TIMEOUT=<larger number>
    export FABRIC_START_TIMEOUT=10
    #echo ${FABRIC_START_TIMEOUT}
    sleep ${FABRIC_START_TIMEOUT}
    
    # Create the channel
    docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel create -o orderer.example.com:7050 -c mychannel -f /etc/hyperledger/configtx/channel.tx
    # Join peer0.org1.example.com to the channel.
    docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel join -b mychannel.block
    ```

13. .../basic-network/ ./start.sh // 셸 스크립트에 의해 docker-compose.yml로 도커 실행환경이 설정되고 컨테이너들이 실행된다.

14. docker ps //로 5개의 컨테이너 확인



### chaincode

해당 주소로 curl 명령어를 쓰면 http get요청을 통해 response로 sh파일을 받게 되고 이를 리눅스가  자동으로 실행하면서 docker image들과 패브릭 구성파일들이 설치된다. 여기에는 예시 체인코드도 제공 되는데 이를 네트워크에 설치해보자

1. docker exec -it cli bash //로 cli의 bash를 실행

2. cli> peer chaincode install -n [이름] -v [버젼] -p [경로] 

   이름과 버젼은 마음대로 설정, 경로는start.sh는 docker-compose.yml 의해 ubuntu와 volume을 공유하고 있다. 따라서 **경로**는 **/opt/gopath/src/github.com/chaincode_example02/node/** 로 주도록한다.

3. cli> peer chaincode list --installed //로 인스톨된 코드를 확인한다.

4. cli> peer chaincode instantiate -n [인스톨한 이름] -v [인스톨한 버젼] -l [컴파일할 언어] -C [채널] -c [체인코드에서 필요로 하는 초기값]

   하이퍼렛저 패브릭은 go(default), node, java 세 가지의 언어를 지원한다. 노드폴더를 인스톨했으므로 **컴파일할 언어**에는 **node**를 적는다.

   채널은 docker-compose.yml에서 채널은 mychannel 로 구성된 것 같다.(추측) start.sh에서 mychannel에 입장하는 코드가 있다. 어쨌든 **채널**에는 **mychannel**을 적어주도록 한다.

   **체인코드에서 필요로 하는 초기값**은 **'{"Args":["init","a",10,"b",20]}'** 을 적어주도록 한다. a를 10의 값으로 b를 20의 값으로 초기화. 궁금한 사람은 ~/HLF/fabric-samples/basic-network 의 3가지 코드를 참조

5. cli> peer chaincode query -n [인스톨한 이름] -c **'{"Args":["query", "a"]}'** -C [채널]

   a값을 읽어 온다.

6. cli> peer chaincode invoke -n [인스톨한 이름] -c **'{"Args":["invoke", "a", "10"]}'** -C [채널]

   a값을 10으로 변경한다.

   -c 이후의 값은 **'{"Args":["체인코드에서 선언한 메서드이름", "해당 메서드에서 필요로 하는 값1","2","3",...]}'** 의 구조이다. 현재 체인코드에서의 메서드 이름이 query와 invoke이므로 체인코드에서 선언한 메서드이름이 query와 invoke가 된 것이다.

   query 명령어는 블록에 기록되지 않고 invoke 명령어는 블록에 기록 된다. 하지만 값을 변경하는 메서드는 invoke 명령어로 실행되어야 한다. query로 실행할 경우 에러는 나지 않지만 변경내용이 적용되지 않는다.(블록과 월드스테이트의 값이 변하지 않는다.)