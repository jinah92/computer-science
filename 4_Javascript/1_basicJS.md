# (Day 1)

Created By: 진아 최
Last Edited: Jan 06, 2020 10:24 AM

- Apache Tomcat ⇒ 웹 서버에 CGI를 입힐 수 있음

Apache만, Tomcat 서버만 설치한다면, 연동 작업이 반드시 필요!

- Tomcat: Java HONE이 항상 기반이 되어야 함 (시스템변수 JAVA_HOME을 아래와 같이 추가)
※ 경로:  `C:\SecureCoding\tools\jdk1.7.0_75`

    ![Day%201/Untitled.png](image/Day%201/Untitled.png)

### 실습

---

1. [tomcat.apache.org](http://tomcat.apache.org) 사이트에서 Tomcat 8버전 다운 (Zip 파일)

    ![Day%201/Untitled%201.png](image/Day%201/Untitled%201.png)

2. 웹사이트 열기 방법
(1) 로컬에 설치된 파일을 직접 여는 방법
(2) 서버 구동 후, 포트로 접속하는 방법

/bin/server.xml 파일에서 아래와 같이 주석처리

![Day%201/Untitled%202.png](image/Day%201/Untitled%202.png)

3. 

![Day%201/Untitled%203.png](image/Day%201/Untitled%203.png)