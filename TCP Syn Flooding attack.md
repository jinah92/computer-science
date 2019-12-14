# (Day 7) 패킷 조작 공격

Created: Dec 11, 2019 10:06 AM
Tags: 3 way handshaking, Scapy, TCP Syn Flooding
Updated: Dec 11, 2019 10:42 PM

## Scapy

- 파이썬으로 작성된 패킷 조작 도구
- 패킷 디코딩, 전송, 캡처, 수정 등의 다양한 기능 제공

### [실습] Scapy를 활용한 3-way handshaking

---

- `sacpy` : scapy 실행
- `ls()` : 패킷 종류 조회

    ![Day%207/Untitled.png](images/Day%207/Untitled.png)

- 예 : `ls(TCP)` : TCP 패킷의 정보 조회

    ![Day%207/Untitled%201.png](images/Day%207/Untitled%201.png)

- `TCP().display()` : TCP 패킷의 상세정보 확인

    ![Day%207/Untitled%202.png](images/Day%207/Untitled%202.png)

- `packet = ip/tcp` :  ip와 tcp를 변수로 받고, 이를 하나의 패킷에 삽입

    ![Day%207/Untitled%203.png](images/Day%207/Untitled%203.png)

- `Sniff()` 스니핑

    ![Day%207/Untitled%204.png](images/Day%207/Untitled%204.png)

    ※ 변수에 지정하는 방법

    ![Day%207/Untitled%205.png](images/Day%207/Untitled%205.png)

- `sf[N].show()` N번째 스니핑한 패킷을 확인

    ![Day%207/Untitled%206.png](images/Day%207/Untitled%206.png)

- **공격 대상에게 패킷 전송**
    - 수신지(destination) ip 주소를 지정

        ![Day%207/Untitled%207.png](images/Day%207/Untitled%207.png)

- tcp 패킷의 송신지 포트 지정 (1024~65535 사이의 랜덤한 수)

    ![Day%207/Untitled%208.png](images/Day%207/Untitled%208.png)

- ip, tcp를 syn(전송할 syn 패킷)에 지정

    ![Day%207/Untitled%209.png](images/Day%207/Untitled%209.png)

- 전송한 syn 패킷에 대한 1개의 응답을 syn_ack 변수로 받고, 수신 패킷 확인

![Day%207/Untitled%2010.png](images/Day%207/Untitled%2010.png)

- ack 패킷 생성 후, 전송 (수신된 syn_ack을 활용, IP 내용은 변경하지 않음)

    ![Day%207/Untitled%2011.png](images/Day%207/Untitled%2011.png)

### [실습] Scapy를 활용한 TCP Syn Flooding

---

- (Kally #1)
 syncookies 사용 안함으로 설정

    `sysctl -a | grep syncookies` syncookie를 사용 여부 확인

    ![Day%207/Untitled%2012.png](images/Day%207/Untitled%2012.png)

    아파치 서버 구동 

    ![Day%207/Untitled%2013.png](images/Day%207/Untitled%2013.png)

- (Kally #2)
iptables 방화벽 설정
- 나가는 tcp 패킷은 RST (패킷 버림)

    ![Day%207/Untitled%2014.png](images/Day%207/Untitled%2014.png)

    sacpy에서 Kalli #1에게 패킷 전송 (loop)

    ![Day%207/Untitled%2015.png](images/Day%207/Untitled%2015.png)

- (Kally #1)
수신한 syn 패킷 확인
- Kally #1의 IP(192.168.67.129)에서 다른 포트로 많은 패킷이 전송된 것을 확인

    ![Day%207/Untitled%2016.png](images/Day%207/Untitled%2016.png)

    - 와이어샤크

    ![Day%207/Untitled%2017.png](images/Day%207/Untitled%2017.png)