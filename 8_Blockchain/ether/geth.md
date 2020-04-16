# 사설망에서 geth 실행

1. geth 초기화
    
    - geth —datadir [private_net 경로] init [genesis.json 경로]
2. geth 콘솔 실행
    
    - geth --networkid "10" --nodiscover --datadir D:\temp\geth\private_net --rpc --rpcaddr "localhost" --rpcport "8545" --rpccorsdomain "*" --rpcapi "eth, net, web3, personal" --targetgaslimit "20000000" console 2>> D:\temp\geth\private_net\error.log
3. 외부 계정 주소 만들기
    
    - personal.newAccount("<비밀번호>")
4. 외부 계정 주소 확인
    
    - eth.accounts
5. 코인베이스 계정 주소 확인
    - eth.coinbase (계정 주소의 인덱스 0)

    코인베이스 계정 주소 : 블록을 생성하려고 채굴했을 때 보상을 받는 계정 주소

6. 코인베이스 계정 주소 변경
    
    - miner.setEtherbase(eth.accounts[인덱스])
7. 제네시스 블록 확인
    
    - eth.getBlock(0)

### 채굴 시작

---

1. 채굴 시작
    - miner.start(1)
2. 채굴 종료
    - miner.stop()
3. 채굴 확인
    - eth.mining ⇒ true (채굴 중)

### 코인베이스 잔액 확인

---

- eth.getBalance(eth.accounts[인덱스])