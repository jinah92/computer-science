# Encoding



# Command Injection

## 공격코드 : "& ipconfig

- 서버측에서 ipconfig 실헹
⇒ 서버의 ip 정보를 탈취 등 (매우 위험한 공격기법 중 하나)
- & ⇒ (웹서버) 파라미터 구분, (command) AND 연산
- 웹서버에서 &을 인코딩한 후 전송 (공격코드의 유실을 방지)

## Encoding (인코딩)

---

### 1. URL Encoding

- 대상 : 클라이언트에서 서버쪽으로 보내는 데이터  (요청 데이터)
- 메타문자 = 약속 문자
- 공격에 악용될 수 있음

### 2. HTML Encoding

- 대상 : 서버에서 클라이언트쪽으로 보내는 데이터 (응답 데이터)
- 방어 목적으로 사용 (XSS)

### 3. Base64 Encoding

- 대상 : 클라이언트와 서버 간 양방향 통신 데이터 모두
- binary 데이터(ex. 동영상)를 text 데이터로 치환
- 기본 인증 ⇒ 취약한 인코딩 방법

    해커가 스니핑 ⇒  인코딩/디코딩을 통해 데이터 내용을 확인할 수 있음

## 코드 예제

---

### (서버 side) URL 디코딩  - URLDecoder.decode();

### (서버 side) HTTP 인코딩 - data.replaceAll( target, result);

```java
public String testXss(HttpServletRequest request) {
		StringBuffer buffer=new StringBuffer();
		String data=request.getParameter("data");
		//replaceAll : String 객체의 메소드
		data = URLDecoder.decode(data);
		String regex = "(?i)<script>";
		Pattern p = Pattern.compile(regex);
		Matcher m = p.matcher(data);
		if(m.find()){	// matches : 비교하려는 문자열이 패턴과 완벽히 일치하면 true, 아니면 false
							// find : 비교하려는 문자열이 패턴을 포함하고 있으면 true, 아니면 false
			data = data.replaceAll("<", "&lt;").replaceAll(">", "&gt;"); //HTML Encoding			
		}	
		buffer.append(data);
        return buffer.toString();	
	}
```