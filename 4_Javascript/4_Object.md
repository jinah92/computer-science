# 자바스크립트 - Object(객체)

### 개요

---

- 자바스크립트에서의 자료형
    1.  기본자료형(Primitive data type) : 한 값
    2. 객체형(object) : 하나 이상의 특성들을 그룹핑

- 객체모델링
    - 객체들의 특성은 특성(property)와 메서드(method)들로 모델링해서 표현
    - 객체 상수 또는 객체생성자 함수로 표현

- 객체모델링 방법
    1. 객체 상수 모델링

        ```javascript
        {//객체 속성 정의
        	number: 1234,
        	model: k5,
        	color: black,
        	...
        
        //객체 메서드 정의
        	start: function(){..},
        	drive: function(){...},
        	...
    }
        ```

        
    
    2. 객체생성자 함수 모델링
    
        ```javascript
        function Car(number, model, color, cc){
        //객체 속성 정의
        	this.number=number;
        	this.mode=model;
        	this.color=color;
        	this.cc=cc;
        
        //객체 메서드 정의
        	this.start=function(){...};
        	this.drive-function(){...};
        }
        ```
    
        

### 자바스크립트 객체

---

- 크게 시스템 생성 객체(이미 존재)와 사용자 생성 객체(사용자들이 코드로 생성)로 구분
- (예) 시스템 생성 객체 : BOM객체, DOM객체
- 사용자 생성 객체 ⇒ 내장 객체 & 사용자 정의 객체
    - 내장 객체: 내부 객체생성자(`Object()`, `Date()`, `Array()` 등)을 이용해서 생성한 객체
    - 사용자 정의 객체: 사용자들이 직접 필요한 객체들을 정의해서 생성
    (사용자 정의 싱글톤 객체, 사용자 정의 객체생성자를 이용해서 생성한 객체)

### 자바스크립트에서의 객체 생성 방법 ① 싱글톤 객체 ② 인스턴스 객체

---

- 싱글톤 객체 생성
    1. 객체 상수 이용 (객체 리터럴)
        - `var 객체변수명 = 객체상수;`  (싱글톤 객체 생성)
    2. `object()` 객체생성자 함수 이용
        - `var 객체변수명 = new Object();` (빈 객체 생성)
        - 빈 객체를 생성 후, 필요할 때마다 속성과 메서드를 추가해서 지정
- 인스턴스 객체 생성
    1. 내장 객체생성자 함수 이용
        - `var 객체변수명 = new 내장_객체생성자();`
    2. 사용자정의 객체생성자 함수 이용
        - `var 객체변수명 = new 사용자정의_객체생성자();`
        - 예약어 `this`를 이용해서 속성과 메서드를 나타낸다.
        (`this` : 현재 코드를 실행하는 객체, 객체생성자에서 생성되는 객체 자신을 나타냄)

### 객체 참조

---

- 객체의 속성 참조 : ① `객체명.속성명` ② `객체명["속성명"]`
- 객체의 메서드 참조 : `객체명.메서드명`

객체생성자를 이용하여 객체 생성
⇒ 각 객체는 속성값과 메서드들을 저장하는 메모리들을 개별적으로 확보

- 프로토타입으로 객체들이 메서드를 공유함
- 생성된 객체를 소멸되는 방법은 없음. 하지만, 사용되지 않는 객체들은 자동으로 수거하는 가비지 컬렉션 기능을 제공함

### 내장 객체

---

- 대표적인 내장 객체들 : `Date` ,`Math`, `Array` 등
1. `Date` 객체
    - 년, 월, 일, 요일, 시간 등의 날짜와 시간 정보를 나타내는 객체
    
- 객체생성자 `Date()`를 이용하여 생성
    
        ```javascript
        var d1 = new Date();
        var d2 = new Date(2018, 0, 1, 0, 0, 1);
        var d3 = new Date(500000000000);
        var d4 = new Date(2000, 12, 25);
        
        document.write(d1 + "<br>");
        document.write(d2 + "<br>");
    document.write(d3 + "<br>");
        document.write(d4 + "<br>");
    ```
    
        
    
    - 현재 시간과 새해까지의 남은 일수 계샨
    
        ```javascript
        var cYear = new Date(); nYear = new Date(2021, 0, 1);
        
        year = cYear.getFullYear();
        month = cYear.getMonth();
        date = cYear.getDate();
        day = cYear.getDay();
        hour = cYear.getHours();
        minute = cYear.getMinutes();
        
        switch(day){
            case 0: days = "일"; break;
            case 1: days = "월"; break;
            case 2: days = "화"; break;
            case 3: days = "수"; break;
            case 4: days = "목"; break;
            case 5: days = "금"; break;
            case 6: days = "토";
        }
    
        leftedTime = nYear.getTime() - cYear.getTime();
        leftedDays = Math.floor(leftedTime/(24*60*60*1000));
        document.write("오늘은" + year + "년 " + month + "월 " + date + "일 (" + days + "요일) 이고, <br>");
        document.write("현재 시간은 " + hour + "시 " + minute + "분 입니다.<hr>");
        document.write("새해 첫날까지는 " + leftedDays + "일이 남았습니다.<br>");
        ```
    
    
    
2. Math` 객체

    - 그 자체가 객체 ⇒ 직접 Math 객체의 속성과 메서드들을 참조해서 사용

    - 예제 1 (수학 상수 및 계산)

        ```javascript
        document.write("<h3>수학 상수들</h3>");
        document.write("오일러 상수: " + Math.E + "<br>");
        document.write("PI(파이): " + Math.PI + "<br>");
        document.write("2의 제곱근: " + Math.SQRT2 + "<br>");
        document.write("2의 자연로그: " + Math.LN2 + "<br>");
        document.write("10의 상용로그: " + Math.LN10 + "<br>");
        document.write("base 2 logarithm of E: " + Math.LOG2E + "<br>");
        
        document.write("<h3>수학 함수 계산</h3>");
        document.write("round(4.7) = " + Math.round(4.7) + "<br>");
        document.write("pow(8, 2): " + Math.pow(8, 2) + "<br>");
        document.write("sqrt(64): " + Math.sqrt(64) + "<br>");
        document.write("abs(-4.7): " + Math.abs(-4.7) + "<br>");
        document.write("ceil(4.4): " + Math.ceil(4.4) + "<br>");
        document.write("floor(4.7): " + Math.floor(4.7) + "<br>");
        document.write("sin (Math.PI/2): " + Math.sin(Math.PI/2) + "<br>");
        document.write("pow(7, 2): " + Math.pow(7, 2) + "<br>");
        
        max = Math.max(0, 99, 50, 70, 10, 30);  //99
        min = Math.min(0, 99, 50, 70, 10, 30);  // 0
        document.write("max: " + max + "<br>");
        document.write("min: " + min + "<br>");
        ```

        

    - 예제 2 (행운번호 생성)

        ```javascript
        <body>
            <h3>행운번호 추천</h3>
            <button onclick="luckyNumber();">행운번호 생성</button>
        
            <script>
                function luckyNumber(){
                    var num = new Array();
                    for(i=1; i<=5; i++){
                        num[i] = Math.floor(Math.random() *44) + 1; //0~45까지의 랜덤 수 생성
                        for(j=1; j<=i; j++){
                            if (num[j]==num[i]){
                                num[i] = Math.floor(Math.random()*44)+1;
                            }
                        }
                    }
                    num[6] = Math.floor(Math.random()*44)+1;
                    for(i=1;j<=5;i++){
                        if (num[j]==num[i])
                            num[6] = Math.floor(Math.random()*44)+1;
                    }
                    document.write("<h3>행운번호</h3><hr>");
                    for(i=1;i<=5;i++)
                        document.write(num[i] + " ");
                    document.write("<br>보너스: " + num[6]);
                }
            </script>
        </body>
        ```
        
        

3. `Array` 객체

    - 여러 데이터들의 순차적 모임을 나타나는 객체

    - 리스트 관리에 필요한 속성과 메서드를 제공

        ```javascript
        var x = [80, 50, 40, 30, 60, 10, 90, 70, 20];
        document.write("(0) 배열 x의 크기(원소개수): " + x.length + "<br>");
        document.write("(1) x의 출력(1): " + x + "<br>");
        document.write("(2) x의 오름차순 정렬: " + x.sort() + "<br>");
        document.write("(3) x의 출력(2): " + x + "<br>");
        document.write("(4) x의 내림차순 정렬: " + x.reverse() + "<br>");
        
        subX = x.slice(2, 5);
        document.write("(5) subX 출력: " + subX + "<br>");
        
        var season1 = ["봄", "여름", "가을", "겨울"];
        var season2 = ["춘곤기", "환절기"];
        seasons = season1.join("=>");
        season3 = season1.concat(season2);
        season4 = season2.concat(season1);
        document.write("(6) 계절 리스트(1): " + seasons + "<br>");
        document.write("(7) 계절 리스트(2): " + season3 + "<br>");
        document.write("(8) 계절의 변화: " + season4 + "<br>");
        document.write("(9) '가을'의 배열 인덱스: " + season1.indexOf('가을') + "<br>");
        ```
        
        

4. `String` 객체
    - 자바스크립트는 2가지 방법으로 문자열 표현이 가능

    - replace(), toUpperCase() 등으로 문자열 값들의 변경 처리는 가능하나, String 객체가 가지고 있는 문자열 값 자체는 변경되지 않음.

    - (예제) 문자열 표현 방법

        ```javascript
        var str1 = "javascript";
        var str2 = new String("web programming");
        
        document.write("typeof(str1): " + typeof(str1) + "<br>");
        document.write("typeof(str2): " + typeof(str2) + "<br>");
        ```

        

    - (예제) String 객체의 속성 및 메서드

        ```javascript
        str1 = "JAVASCRIPT";
        str2 = "web programming";
        str3 = "컴퓨터 개론";
        str4 = "자료구조, 데이터베이스, 알고리즘, 운영체제";
        
        document.write("alogrithm의 문자 개수: " + str1.length + "<br>");
        document.write("컴퓨터개론의 문자 개수: " + str3.length + "<hr>");
        document.write("문자열 변환 (대문자): " + str2.toUpperCase() + "<br>");
        document.write("문자열 변환 (소문자): " + str1.toLowerCase() + "<hr>");
        str5 = str4.replace("데이터베이스", "웹 프로그래밍");
        document.write("str5 문자열: " + str5 + "<br>");
        document.write("str4 문자열: " + str4 + "<hr>");
        
        document.write("[배열 subject 출력]<br>");
        subject = str4.split(',');
        for(i=0; i<subject.length; i++)
        document.write("subject[" + i + "]=" + subject[i] + "<br>");
        ```
        
        

5. `Number` 객체
    - number 형 자료들을 객체로 표현한 것

    - 수치의 유효자릿수 표현 관련된 메서드를 지원

    - (예제) `Number` 객체 특성

        ```javascript
        document.write("전역 내장 함수<hr>");
        document.write("Number(true): " + Number(true) + "<br>");                   // 1
        document.write("Number(false): " + Number(false) + "<br>");                 // 0
        document.write("Number('50'): " + Number('50') + "<br>");                   // 50
        document.write("Number(' 50'): " + Number(' 50') + "<br>");                 // 50
        document.write("Number('50 '): " + Number('50 ') + "<br>");                 // 50
        document.write("Number('50 80'): " + Number('50 80') + "<br>");             // NaN
        document.write("Number('June'): " + Number('June') + "<hr>");               // NaN
        document.write("parseInt('80'): " + parseInt('80') + "<br>");               // 80
        document.write("parseInt('80.56'): " + parseInt('80.56') + "<br>");         // 80
        document.write("parseInt('50 70 90'): " + parseInt('50 70 90') + "<br>");   // 50
        document.write("parseInt('80점'): " + parseInt('80점') + "<br>");           // 80
        document.write("parseInt('점수 80'): " + parseInt('점수 80') + "<br>");     // NaN
        
        document.write("parseFloat('80'): " + parseFloat("80") + "<br>");           // 80
        document.write("parseFloat('80.56'): " + parseFloat("80.56") + "<br>");     // 80.56
        document.write("parseFloat('50 70 90'): " + parseFloat("50 70 90") + "<br>");// 50
        document.write("parseFloat('80점'): " + parseFloat('80점') + "<br>");       // 80
        document.write("parseFloat('점수 80'): " + parseFloat('점수 80') + "<br>"); // NaN
        
        document.write("수치 상수<hr>");
        var x1 = Number.MAX_VALUE;
        var x2 = Number.MIN_VALUE;
        var x3 = Number.NaN;
        var x4 = Number.NEGATIVE_INFINITY, x5 = Number.POSITIVE_INFINITY;
        document.write("Number.MAX_VALUE: " + x1 + "<br>");
        document.write("Number.MIN_VALUE: " + x2 + "<br>");
        document.write("Number.NaN: " + x3 + "<br>");
        document.write("Number.NEGATIVE_INFINITY: " + x4 + "<br>");
        document.write("Number.POSITIVE_INFINITY: " + x5 + "<br>");
        ```
        
        

6. `Object` 객체
    - 모든 객체들의 최상위 객체

    - 자바스크립트의 모든 객체들은 Object 객체의 모든 속성과 메소드를 상속받고, 필요하면 재정의 할 수 있음

    - 주요 메서드 : `constructor()`, `hasOwnProperty(name)`, `toString(n)`

    - (예제) toString() 메서드의 재정의, constructor 속성

        ```javascript
        var obj = new Object();
        obj.name = "조민규"; obj.major = "건축공학";
        
        document.write("obj의 constructor: ");
        document.write(obj.constructor + "<br>");  // function Object() { [native code] }
        document.write("obj의 toString(): ");
        document.write(obj.toString() + "<br>");   // [object Object]
        
        //toString() 재정의
        obj.toString = function() {
            document.write("{name: " + this.name + ", major: " + this.major + "}<br>");
        };
        document.write("obj의 toString(): ");
        document.write(obj.toString()); // 재정의 // {name: 조민규, major: 건축공학} undefined
        ```