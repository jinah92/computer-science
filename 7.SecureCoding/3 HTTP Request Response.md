# HTTP Request, Response

Class: Secure Coding
Created: Mar 12, 2020 4:29 PM
Reviewed: No

### 취약한 HTTP Method

- HEAD, PUT, DELETE, TRACE, OPTIONS
- Get, Post 외의 메서드는 보안에 취약하므로 사용하지 않는다

    취약한 메서드는 비활성화(disable) 한 후, 배포

### SOAP

---

- 이기종 간의 B2B 통신 표준 프로토콜 (메시지 표준은 HTTP 프로토콜)
- 웹 API 개발 시, SOAP 표준을 따라야 한다
- XML 기반의 메시지를 컴퓨터 네트워크 상에서 교환하는 프로토콜
- 응답/전송하는 SOAP 메시지의 구조는 동일하다

### REST API

---

- 기존 B2C 웹 통신과 유사하게 B2B 통신을 하는 프로토콜

표준 아님

[권고사항] Get, Post 만 사용