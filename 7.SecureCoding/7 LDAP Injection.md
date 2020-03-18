# LDAP Injection



### LADP (Lightweight Directory Access Protocol)

- 기업에서 사용하는 사용자 관리 프로토콜
- <LDAP://LDAP SERVER IP>; (조건문)

### 공격 예시

---

- <LDAP://ldapserver>;(name=홍길동);mail;);cn,phone,department
- <LDAP://ldapserver>;(name=*);location;nation,mail,cn;);cn,phone,department