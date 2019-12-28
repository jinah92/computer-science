# (Day 7) 방화벽 컴퓨터

### 사설 IP (=Private IP = N=nonroutable IP)

---

- 내부의 사용자는 외부의 인터넷을 사용, 외부의 사용자는 내부로 침입할 수 없게 만드는 방법 중에 가장 보편적으로 사용하는 방법
- 사설 IP 주소 범위
    - 10.0.0.0~10.255.255.255
    - 172.16.0.0~172.31.255.255
    - 192.168.0.0~192.168.255.255

    해당 범위에 있는 컴퓨터는 인터넷 라우터를 통과할 수 없다. 즉, 외부의 컴퓨터는 해당 주소 범위에 있는 컴퓨터에 접근할 수 없다.

### IP 마스커레이딩 (Masquerading)

---

- 사설 IP 주소의 컴퓨터가 외부 인터넷에 접속할 수 있도록 해주는 방법
- 사설 네트워크의 모든 컴퓨터가 외부 인터넷을 사용할 때, 방화벽의 공인 IP를 사용한다.

### NAT, SNAT, DNAT

---

- NAT (Network Address Translation) : ip 패킷 헤더의 ip주소를 변경하는 기능 혹은 절차
- SNAT (Source NAT) : 내부 ⇒ 외부 / 패킷의 source ip 주소를 변경하는 것
※ 사설 IP를 동적으로 운영 ⇒ IP 마스커레이딩
※ 사설 IP를 고정하여 운영 ⇒ SNAT
- DNAT (Destination NAT) : 외부 ⇒ 내부 / 패킷의 destination ip 주소를 변경하여 외부의 컴퓨터가 내부 컴퓨터에 접속
※ 대표적으로, Load Balancer

### 실습 - 방화벽 컴퓨터 만들기

---

[구현할 네트워크 구성](https://www.notion.so/5085a6bbacd845598f3dc8f9920778b6)

1. **VM에서 설정된 네트워크 확인** [Edit] - [Virtual Network Editor]

    ![Day%207/Untitled.png](images/Day%207/Untitled.png)

2. **(Server B) 네트워크 정보를 VMnet0 (Bridged)로 변경하고, IP주소를 변경**
    - [Settings] - [Network connection]에서 [Bridged: Connected ~~]를 선택

        ![Day%207/Untitled%201.png](images/Day%207/Untitled%201.png)

    - Server B의 네트워크 정보를 아래와 같이 변경 `vi /etc/network/interfaces`

        ![Day%207/Untitled%202.png](images/Day%207/Untitled%202.png)

    - reboot 후, 변경된 네트워크 정보를 확인  `ifconfig`

        ![Day%207/Untitled%203.png](images/Day%207/Untitled%203.png)

    - 인터넷 접속을 시도 (⇒ 게이트웨이 10..1.1.1.이 작동되지 않아, 응답이 없음)
    `ping -c www.kernel.org`

3. **(Client) 네트워크 정보를 VMnet0 (Bridged)로 변경하고, IP주소도 고정 IP로 변경**
- IP : 10.1.1.10 , Subnet Mask : 255.255.255.0 , Gateway : 10.1.1.1
    - 터미널에서 `nm-connection-editor` 입력
    - [유선 연결 1] -[편집] - [IPv4 설정] 에서 아래와 같이 입력

        ![Day%207/Untitled%204.png](images/Day%207/Untitled%204.png)

    - Client를 재부팅 (`reboot`) 후, ens33의 IP주소를 확인 (`ifconfig`)

        ![Day%207/Untitled%205.png](images/Day%207/Untitled%205.png)

    - 같은 사설 세트워크에 연결된 Server(B)와의 연결 확인 `ping -c [패킷 수] [IP주소]`

        ![Day%207/Untitled%206.png](images/Day%207/Untitled%206.png)

4. **(Server) 방화벽 컴퓨터로 운영하려는 Server 설정**
    - `nm-connection-editor` 로 [유선 연결 1] 장치를 확인 (ens32)

        ![Day%207/Untitled%207.png](images/Day%207/Untitled%207.png)

    - VMnet0 (Bridged) 를 이용한 ens35 장치를 설치가 필요하다. 우선 `halt -p`로 Server 종료

5. **Serverdp VMnet0 (Bridged) 를 이용한 랜카드를 하나 더 추가 후, 서버 부팅**
    - [Settings] - 왼쪽 하단의 [Add]
    - [Network Adapter]를 선택
    - 새로 추가된 Network Adatper(랜카드)에 [Bridged]를 선택

        ![Day%207/Untitled%208.png](images/Day%207/Untitled%208.png)

        ![Day%207/Untitled%209.png](images/Day%207/Untitled%209.png)

    - Server 부팅
6. **(Server) 장착한 랜카드 VMnet0 (Bridged)를 Server의 ens35 장치로 인식시키기**
    - [시스템 설정] - [네트워크]
    - 두 번째 유선에서 '켬'을 확인하고, [옵션]을 클릭

    ![Day%207/Untitled%2010.png](images/Day%207/Untitled%2010.png)

    ![Day%207/Untitled%2011.png](images/Day%207/Untitled%2011.png)

    - [IPv4 설정] 텝에서 방식을 '수동'으로 바꾸고, 주소와 넷마스크를 아래와 같이 설정
    - 재부팅 후, `ifconfig`를 통해 장치 확인

        ![Day%207/Untitled%2012.png](images/Day%207/Untitled%2012.png)

7. **(Server) 방화벽 컴퓨터에 정책을 적용**
    - 사설 네트워크 컴퓨터들이 외부 인터넷을 이용하도록 마스커레이드를 설정
        1. vi 에디터로 `/etc/sysctl.conf` 파일을 열고, #net.ipv4.ip_forward=1의 주석(#) 제거

            재부팅되어도 ip_forward 값이 항상 1(ON)으로 설정됨

        2. `echo 1 > /proc/sys/net/ipv4/ip_forward` 입력
        (앞으로 설정할 내용을 바로 IP 포워딩 되도록 설정)
        3. `cat /proc/sys/net/ipv4/ip_forward` 입력 시 결과가 1이 나오는지 확인
        4. 터미널에서 다음 명령을 입력 (FORWARD, INPUT, OUTPUT의 기본 정책을 DROP으로 설정 = 초기화)
        iptables --policy FORWARD DROP
        iptables --policy INPUT DROP
        iptables --policy OUTPUT DROP

    - 내부의 사설 네트워크와 연결되는 장치(ens35)와 외부 인터넷과 연결되는 장치(ens32)에 모든 패킷이 통과되도록 설정 (`iptables`)
        - iptables --append INPUT --in-interface [내부사설네트워크와 연결되는 네트워크 장치] --source 10.1.1.0/24 --match state --state NEW,ESTABLISED --jump ACCEPT
        - iptables --append OUTPUT --out-interface [내부사설네트워크와 연결되는 네트워크 장치] --destination 10.1.1.0/24 --match state --state NEW,ESTABLISED --jump ACCEPT
        - iptables --append FORWARD --in-interface [내부사설네트워크와 연결되는 네트워크 장치] --source 10.1.1.0/24 --destination 0.0.0.0/0 --match state --state NEW,ESTABLISED --jump ACCEPT
        - iptables --append FORWARD --in-interface [외부 인터넷과 연결되는 네트워크 장치] --destination 10.1.1.0/24 --match state --state ESTABLISED --jump ACCEPT

    - 외부 인터넷으로 연결되는 장치(ens32)에 마스커레이드를 허가 (⇒ 내부 사설 네트워크와 연결되는 장치인 ens35와 연결된 모든 컴퓨터는 외부와 인터넷이 가능하도록 연결)
        
        - iptables --table nat --append POSTROUTING --out-interface [외부 인터넷과 연결되는 네트워크 장치] --jump MASQUERADE
    - 지끔까지 설정한 내용 저장 `iptables-save > /etc/iptables.rules`
8. **(Client) 웹 브라우저를 실행해 인터넷을 사용**
    
    - Client(10.1.1.10) ⇒ 게이트웨이(10.1.1.1) ⇒ 방화벽 공인 IP(192.168.111.100) ⇒ 외부 인터넷
9. (Winclient) Client에서 외부의 Winclient로 접속하기 위해, Winclient에 FTP 서버를 설치하고 접속 테스트
    - FileZilla Server 프로그램 다운로드 [http://filezilla-project.org/](http://filezilla-project.org/)
    - 기본값으로 설치 후, 관리자의 암호설정 & [Always connect ~] 부분 체크
    - [Edit] - [Users]에서 ubuntu 유저를 추가
    - [Shared folders] 에서 파일을 공유할 폴더를 지정, 모든 권한 허용
    - 관리자 프롬포트에서 Windows 방화벽을 잠시 꺼두기
    `netsh advfirewall set allprofiles state off`
    - ipcofig 로 IP주소를 확인

10. (Client) 방화벽(Server)를 통해서 외부의 Winclient로 접속되는지를 확인
    - gftp 클라이언트를 설치 `apt-get -y install gftp`
    - `gftp` 실행하여, FTP 서버에 접속
    (호스트 ⇒ Winclient 주소, 사용자/비밀번호 ⇒ ubuntu )

        ![Day%207/Untitled%2013.png](images/Day%207/Untitled%2013.png)

11. (Winclient) 명령 프롬포트에서 `netstat /an` 으로 확인
    - **Client의 주소는 10.1.1.10이 아닌, Server(방화벽)의 ens32 주소인 192.168.111.100으로 확인**
    ⇒ FTP 서버는 Client의 IP 주소를 알 수 없음

        ![Day%207/Untitled%2014.png](images/Day%207/Untitled%2014.png)

12. (Server B) Server B를 웹서버로 운영하기 위해, httpd를 설치
    - 웹 서버 설치 `apt-get -y install apache2`
    - 방화벽에서 웹 서버를 허용 `ufw allow http`
    - /var/ww/html 디렉토리에 index.html 파일을 새로 생성하고, 적절한 내용을 넣어 저장함.
    - `systemctl restart apache2`  웹 서버를 재시작
    `systemctl enable apache2`  웹 서버를 상시 가동

13. (Server) 외부(Winclient)에서 공인 IP주소인 192.168.111.100으로 웹 서비스를 요청하면, 사설 네트워크 내부에 있는 웹서버 ServerB로 연결되도록 서버를 설정
    - 터미널에서 다음 명령을 입력
    iptables --tables nat --append PREROUTING --proto tcp --in-interface [외부네트워크장치] --dport [포트번호] --jump DNAT --to-destination [내부의 웹서버 사설ip]
    - 추가한 방화벽 정책 내용을 저장 `iptables-save > /etc/iiptables.rules`

14. (Winclient) 방화벽의 공인 ip주소 192.168.111.100 으로 웹페이지에 접속
※ 접속이 안되면 Server에서 `ufw disable` 명령어 수행 후, 다시 시도
    - Winclient (192.168.111.128) ⇒ Server ens32 (192.168.111.100) ⇒ Server ens35 (10.1.1.1) ⇒ Server B (10.1.1.20)

    ![Day%207/Untitled%2015.png](images/Day%207/Untitled%2015.png)