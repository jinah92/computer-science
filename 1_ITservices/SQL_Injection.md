# SQL Injection

## 개요

---

외부 입력값에 쿼리 조작 문자열 포함 여부를 확인하지 않고 쿼리문을 생성 및 사용하는 경우에 발생

 **→ 쿼리문의 구조와 의미가 변형되어 실행**

 **⇒ 권한 밖의 데이터에 접근 가능**

 **⇒ DBMS 서버의 제어권 탈취**

 **⇒ 쿼리 실행 결과를 우회한 처리** 

## 공격 방식

---

### 정상 입력

- search.jst?id=1234
⇒  select * from users where id = 1234 (id 컬럼값이 1234인 사용자의 정보를 조회)

### 조작된 입력

1.  **항상 참이되는 입력** ⇒ 모든 데이터(권한 밖의 데이터)에 대해 접근 가능
    - search.jsp?id=1234 or 1 = 1
    ⇒ select * from users where id = 1234 or 1 = 1
    → 모든 사용자의 정보를 조회
2. **오류를 유발하는 입력** ⇒ 오류 메시지를 통해 추가 공격에 필요한 정보를 수집
    - search.jsp?id=1234' 
    ⇒ select * from users where id = 1234' 
    → SQL Syntax Error

3.  **Stored Procedure를 호출하는 입력** ⇒ 해당 DBMS의 쉘을 구동 (= 제어권 탈취)

- seacrh.jsp?id=1234; exec xp_cmdshell 'cmd.exe /c dir' ; --
⇒ select * from users where id = 1234; exec xp_cmdshell 'cmd.exe /c dir'; --
→ id 컬럼값이 1234인 사용자의 정보 조회 + DBMS 쉘(명령어 창)에서 dir 명령어를 실행한 결과 출력
→ 일반적으로 시스템에서 제공하는 SP를 우선적으로 호출

4. **UNION 구문을 이요한 호출** ⇒ 공격자가 생성한 쿼리의 결과가 사용자 화면을 통해 노출(유출)

- search.jsp?id=1234 UNION select ... 
→ 일반적으로 시스템에서 제공하는 테이블을 우선적으로 호출

    [select * from users where id =1234](https://www.notion.so/8c74a7f24627437ebd5fb5af5378b4bc)

    [UNION select ...](https://www.notion.so/fa709d1de8424fe5be08a90d7c28fd8f)

5. **참, 거짓에 따른 서버의 반응** = Blind SQL Injection

- 정상적인 입력과 처리
search.jsp?id=1234 ⇒ 회원 정보가 조회
search.jsp?id=9999 ⇒ 회원 없음 메시지
- 조작된 입력
search.jsp?id=1234 and 1 = 1
⇒ select * from users where id = 1234 and 1 = 1
→ 회원 정보 조회
search.jsp?id=1234 and 1 = 2
⇒ select * from users where id = 1234 and 1 = 2
→ 회원 없음 메시지

search.jsp?id=1234 and (selet ... )
⇒ select * from users where id = 1234 and (select ...)
→ 공격자가 추가한 (select ..) 쿼리의 결과가 참이면 회원 정보가 조회, 거짓이면 회원 없음 메시지 출력

### 대응 방안

- **정적 쿼리를 사용 및 실행**
  ⇒ PreparedStatement 객체 사용
  ⇒ 구조화된 쿼리 실행
  ⇒ 파라미터화된 쿼리 실행

- **동적 쿼리를 사용해야 하는 경우, 입력값 검증 후 사용**
  ⇒ 쿼리 조작 문자열 포함 여부를 확인 후 사용

- 어플리케이션에서 사용하는 DB 사용자에게 최소 권한을 부여
  ⇒ Stored Procedure을 이용한 공격 또는 UNION 기반 공격 완화

- **오류 메시지에 시스템 정보가 노출되지 않도록 통제**
  ⇒ Error 기반 공격 완화

- **ORM 프레임워크 사용하는 경우, 외부 입력값을 쿼리 맵에 바인딩할 때 # 기호를 이용**

  