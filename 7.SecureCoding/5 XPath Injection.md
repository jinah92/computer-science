# XPath Injection



## XPath = XML Path

## 방어 방법

---

1. 필터링
    
    - 필터링 대상 문자 : ( ) = ' [ ] : , * /
    
2. XQuery API 사용하여 정적 구조로 쿼리 작성 (표준 API 아님)

    ```java
    System.out.println("filter 전" + name);
    name = name.replaceAll("[()='\\[\\]:,*/]", "");
    System.out.println("filter 후" + name);
    ```
    
    
    
    