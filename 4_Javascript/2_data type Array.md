# 자바스크립트(기본) - data type, Array

Created By: 진아 최
Last Edited: Jan 13, 2020 1:10 PM

- 자료형
    - 개요
    1. 기본자료형 (Primitive data type) : 단순히 한 값으로 표현
    2. 객체자료형 (Object data type) : 여러 속성값 및 동작들을 그룹핑해서 표현

    자바스크립트 자료형 (6가지)
    - String, number, boolean, object, function, undefined

- String 형 ( = 문자열)
    - 일련의 문자로 표현되는 자료들
    - 내부적으로 시작인덱스가 0인 문자 배열로 저장 (length 속성으로 확인)
- Number 형 ( = 숫자)
    - 정수 또는 실수 등의 수치값
    - 예약어 : NaN(수치형아 아님), Infinity, -Infinity (시스템에서 표현가능한 최대/최소값을 벗어남)
- boolean 형 (참 또는 거짓)
    - 관계식 또는 논리식으로 표현되고, 조건문과 반복문에서 실행조건을 표현하는 데 사용됨
    - 0, -0, "", underined형, null객체 ⇒ false 이고, 나머지는 true

- undefined 형
    - 변수가 자료형을 결정할 수 없음
    - 변수를 선언했지만 아직 아무런 값도 저장되지 않거나, 변수값으로 예약어 undefined를 저장한 변수들의 자료형

    null : object 자료형 
    undefiend : undefiend 자료형

- typeof 연산자와 형변환
    - 피연산자의 자료형을 반환
    - (자동 형변환) 자료형이 문자열과 수치형으로 서로 다른 경우
        - +연산자 ⇒ , 수치형을 문자열형으로 자동변환 후 연산 수행
        - -, *, / 연산자 ⇒ 피연산자들을 수치형으로 자동변환 후 연산 수행

- JVM은 4 byte 단위로 처리 **(기본자료형 : int)**

[Data Type (Java)](https://www.notion.so/97c9f39d5096460ab4dbc8e22ffe7f19)

[Data Type (Javascript)](https://www.notion.so/f4e670e493f645f78be080883d2efbd8)

- 로컬변수 ⇒ Stack 메모리에 위치
- Java 데이터 할당
    1. [Test.java](http://test.java) 
    2. Test.class
    3. JVM Load
    4. (main 제외) static 멤버 초기화
    5. 상속관계 파악 (⇒ JVM Load)
    6. main 수행

- 이벤트 속성
    - 이벤트 : 사용자의 마우스 클릭, 키보드 입력, 이미지 파일 로딩 등과 같이 웹 문서를 로드한 웹 브라우저에서 발생되는 어떤 동작 및 상태변화
    - 이벤트핸들러 : 이벤트가 발생할 때 실행되는 코드
    이벤트 등록 : 이벤트와 이벤트핸들러를 연결하는 과정
    - `click, mouseover, change, load, resize`
- 입력 함수
    - 메시지 대화상자를 통해 사용자들에게 직접 문자열 자료를 입력받는 방법
    - `alert()` : 대화상자를 출력하고 사용자가 '확인' 버튼을 누르면 대화상자를 종료
    - `confirm()` : 대화상자를 통해 사용자에게 확인 또는 취소의 응답을 요구하는 메시지를 제공하고, 응답을 받아 반환
    - `prompt()` : 대화상자를 통해 사용자에게 문자열의 입력을 요구하고 사용자가 입력한 문자열을 반환        ※ 수치값을 입력하여도, 문자열로 반환됨

- 제어문
    - 순차 구조: 명령문들을 순차적으로 나열 (별도의 제어문 없음)
    - 선택문: if, if~else, if~else if, switch
    - 반복문: while, for, do~while, for-in
    - 기타 : continue, break

- 배열 (Array)
    - 서로 관련된 여러 변수 메모리들을 모아놓은 것
    - 생성 방법
        1. 리터럴 나열 (모든 배열 원소값을 나열)
        `var a1 = [10, 20, 30, 40, 50];`
        2. 배열 객체 생성자 함수 사용 `Array()
        var b1 = new Array(10, 20, 30, 40, 50);`

    - 자바스크립트에서의 배열 특징
        1. 미리 정의하지 않고 사용 가능
        2. 배열 크기도 동적으로 변경 가능
        3. 배열 원소들의 자료형은 같지 않아도 됨
        4. 문자열 인덱스(named index, key)를 이용해서 배열원소에 접근 가능

        정수, 문자열 인덱스를 혼용 시, (⇒ for-in 문으로 모두 접근 가능)
        배열 크기는 정수 인덱스 배열원소들만을 고려해서 계산됨
        (문자열 인덱스 배열원소는 제외)

    - for-in문
        - 배열 또는 객체의 모든 자료값들을 빠짐없이 참조 가능
        - 정수 인덱스를 가진 요소 ⇒ 문자열 인덱스를 가진 요소 순으로 출력
        - 정수 인덱스를 가진 배열원소가 undefined 형이면, 무시하고 출력하지 않음