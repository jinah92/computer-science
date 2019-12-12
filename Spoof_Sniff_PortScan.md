# (Day 6) 네트워크, ARP Spoofing, Sniffing, Port Scanning

Created: Dec 10, 2019 1:22 PM
Tags: ARP, ARP Spoofing, DHCP, DNS, Ethernet, IP, MAC, Routing, Subnet Mask, 네트워크
Updated: Dec 11, 2019 10:53 PM

![Day%206%20ARP%20Spoofing%20Sniffing%20Port%20Scanning/Untitled.png](Day%206%20ARP%20Spoofing%20Sniffing%20Port%20Scanning/Untitled.png)

## 네트워크 기초

---

- 이더넷 어뎁터 = LAN 카드 = NIC
- **Ethernet**
⇒ LAN영역에서 사용하는 통신 기술 중 하나
⇒ LAN영역에서 사용하는 기술 중 사실상의 표준 (De Facto Standard) 방식
- **IPv4 주소**
⇒ 총 32bit (0.0.0.0 ~ 255.255.255.255)로 구성된 주소 체계
⇒ 주소 표현 개수 :  2^32개 (2의 32승)
- **IP (Internet Protocol)**
⇒ 인터넷 공간에서 PC가 사용하는 고유한 식별자
    - IP 주소의 클래스(등급) → IP 주소의 첫번째 자리 범위
        - A 클래스 : 1 ~ 126       = 0000 0001 ~ 0111 1110
        - B 클래스 : 128 ~ 191   = 1000 0000 ~ 1011 1111
        - C 클래스 : 192 ~ 223   = 1100 0000 ~ 1101 1111
    - 구글에서 제공하는 DNS 서버의 IP 주소 = 8.8.8.8 (⇒ A 클래스)
    - KT에서 제공하는 DNS 서버의 IP 주소 = 168.126.63.1 (⇒ B 클래스)
    - 127.0.0.1 → 어떤 클래스에도 속하지 않음 (⇒ 자기가 사용하는 LAN카드 자신을 의미(loopback address)
- **서브넷 마스크 (subent mask)**
⇒ IP 주소를 서브넷 마스크를 이용해 표기하는 방식
⇒ IP 주소를 네트워크 ID와 호스트 ID로 구분
    - IP                        Subent Mask          Network ID             Host ID
                                                            = 국번                    = 전화번호
    10.10.10.10         255.0.0.0                 10                           10.10.10
- **게이트웨이(getway) = 라우터(router) ⇒ 각기 다른 네트워크 ID를 사용하는 LAN영역을 연결**
————————-      ——————-
SW 측면 강조                HW 측면 강조
- **LAN 영역 (Local Area Network)**
⇒ **동일한 네트워크 ID**를 공유하는 장치들의 집합
⇒ **동일한 게이트웨이 주소**를 사용하는 장치들의 집합
- **라우팅(routing) :** 다른 네트워크 ID를 사용하는 LAN 영역을 연결
- **스위칭(switching)** : LAN 영역에서 MAC주소에 기반한 내부 통신

![Day%206%20ARP%20Spoofing%20Sniffing%20Port%20Scanning/Untitled%201.png](Day%206%20ARP%20Spoofing%20Sniffing%20Port%20Scanning/Untitled%201.png)

- **물리적 주소 = MAC 주소**
⇒ LAN카드에 부여된 주소로, LAN 영역에서 내부 통신을 수행하기 위한 주소
⇒ 48 bit = OUI + 일련번호  (* OUI : LAN카드 제조사 고유번호)
- **DHCP (Dynamic Host Configuration Protocol) = 유동 IP 환경**
⇒ 사용할 IP 주소 범위를 서버에 미리 등록
⇒ PC 사용자에게 IP 주소, 서브넷 마스크, 게이트웨이 주소, DNS 주소 등을 자동으로 할당해주는 서비스
- **DNS (Domain Name System) 서버**
⇒ 도메인 이름과 IP 주소의 대응 관계를 데이터베이스 형태로 저장해 사용하는 서버
- IP (32bit) = 네트워크 ID + 호스트 ID ⇒ IP 주소 기반의 Routing
MAC (48bit) = OUI + 일련번호 ⇒ MAC 주소 기반의 Switching

## ARP Spoofing

---

- `ping`
**=> 출발지 호스트(내 PC)와 목적지 호스트 사이의 회선 연결 상태나 목적지 운영체제의 동작 여부를 점검하기 위한 도구**

![Day%206%20ARP%20Spoofing%20Sniffing%20Port%20Scanning/Untitled%202.png](Day%206%20ARP%20Spoofing%20Sniffing%20Port%20Scanning/Untitled%202.png)

## Spoofing (DNS Spoofing)

---

**MTM attack (Man In The Middle)**
⇒ 두 호스트 간의 통신을 하고 있을 때, 중간자가 사이에 끼어들어 통신 내용을 토청 및 조작하는 공격

- **ettercap**
    - LAN 환경에서 중간자 공격을 수행할 수 있도록 구현한 프로그램
    - GUI 제공
    - 다양한 플러그인 제공

## Port Scanning (nmap 툴을 활용)

---

- 타겟 서버의 포트 상태를 확인
- nmap : 네트워크에 연결되어 있는 호스트의 정보를 파악하는 도구
    - 네트워크에 연결되어 있는 **호스트의 IP, OS**
    - **열려있는 포트**
    - **서비스하는 소프트웨어 버전**  등

### TCP Open Scan

- **정상적인 TCP 3-way Handshaking 과정**을 통해서 사용 중인 포트를 확인
    - 포트가 열린 경우, **SYN → SYN/ACK → ACK**
    - 포트가 닫힌 경우, **SYN → RST/ACK**
    - **연결에 대한 로그가 남는다** ⇒ 공격자 입장에서 안정하지 않은 방법
- `nmap -sT "호스트_IP" "Port_no"`
- 특징 : 빠른 스캔이 가능

### Stealth Scan

- **TCP 3-way handshaking 과정을 거치지 않음** ⇒ **로그가 남지 않음**
- TCP half open scan (= TCP SYN open scan)  **⇒ -sS**
- FIN scan / XMAS scan / NULL scan
    - FIN : FIN                                                  **⇒ -sF**
    - XMAS : FIN, PSH, URG                            **⇒ -sX**
    - NULL : X                                                  **⇒ -sN**