# SQL Injection



## 쿼리 조작

---

### (전체 데이터 조회)
select * from table 
where id= '**' or ''='**' and pw='**' or ''='**'

### (특정 사용자 계정으로 로그인)
select * from table 
where id= 'admin'  — and pw='[아무 값]**'**'

select * from table 
where id= 'admin'#' and pw='[아무 값]**'**'

- 주석처리(—는 언어별 상이 할 수 있음)

# SQL Injection

- 입력 data가 SQL의 구조를 바꾸는 취약점

방어 방법 : SQL를 고정시킴!
 - 

## 1. Form based SQL Injection

---

- 정상 수행을 목표
- (ex) 회원가입 form, 로그인 form, 검색 form 등
- 정상 로직을 우회하여 정상 수행
- '**' or ''='**'

## 2. Error based SQL Injection

---

- 공격자가 에러 메시지를 발생하여 공격 대상 PC의 정보를 취득
- 

## 3. Union based SQL Injection

---

- select 결과를 합산해서 조회

## 4. Stored Procedure SQL Injection

---

- 내장 함수를 이용
- DBMS에서 지원하는 저장 프로시저
⇒ 특정 기능을 미리 구현해 놓고 호출해 사용 (동적SQL 구문보다 높은 성능, 미리 저장된 기능 이용)
⇒ 보안에 취약

## 5. Blind SQL Injection

---

- 쿼리 결과의 참/거짓으로 DB의 정보를 판단하는 방법으로 DB정보를 추출

### SQL Injection 대응 : 정적쿼리 사용

---

![SQL%20Injection/Untitled.png](img/SQL%20Injection/Untitled.png)

![SQL%20Injection/Untitled%201.png](img/SQL%20Injection/Untitled%201.png)

**정적 쿼리를 사용할 수 없는 환경 (Parameterized API 사용불가)**
 ⇒ 필터링 으로 대응 
 ⇒ 화이트리스트 방식의 필터링
 ⇒ 단점 : 융통성이 없는 프로그래밍
 ⇒ 좋은 대응방안이 아니므로, 되도록이면 정적 쿼리를 적용하는 것을 권장