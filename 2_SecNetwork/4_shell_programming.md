# (Day 4) 쉘 스크립트 프로그래밍

### 쉘

- 사용자가 입력한 명령을 해석해, 커널에게 전달하거나 커널의 처리 결과를 사용자에게 전달
- 명령을 입력하는 환경

## 7.1.쉘의 기본

---

### 7.1.1. 우분투의 bash 쉘

- Alias 기능 (명령 단축)
- History

### 7.1.2. 쉘의 명령문 처리방법

### 7.1.3. 환경 변수

- 쉘은 여러 가지 환경 변수 값을 가진다
- 설정된 환경변수는 `echo $환경변수이름` 명령으로 확인
- (예시) `echo $HOSTNAME` : 호스트 이름을 출력
- printenv

## 7.2. 쉘 스크립트 프로그래밍 실습

---

### 7.2.1.  작성 및 실행

- gedit name.sh

    #!/bin/sh    // bash 쉘 사용
    echo "사용자 이름 : " $USER   
    echo "홈 디렉토리 : " $HOME
    exit 0       // 종료 코드 반환

- 실햏방법 1) sh 명령
- 실햏방법 2) 실행가능 속성으로 변경후, 실행

### 7.2.2. 변수

- 기본
    - 변수를 사용하기 전에 미리 선언하지 않음
    - 변수에 넣는 모든 값 ⇒ 문자열 취급
    - 변수 이름은 대소문자를 구분

- **변수의 입출력**

    #!bin/sh
    myvar="Hi Woo"
    echo $myvar
    echo "$myvar"
    echo '$myvar'
    echo \$myvar
    echo 값 입력 :
    read myvar
    echo '$myvar' = $myvar
    exit 0

출력 화면

![Day%204/Untitled.png](images/Day%204/Untitled.png)

- **숫자 계산**
    - `expr` 키워드 사용
    - 수식과 함께 `(백틱)으로 묶기
    - 괄호()와 곱하기(*) 앞에는 역슬래시(\)를 붙이기

    #!bin/sh
    num1=100
    num2=$num1+200       // 문자열로 인식
    echo $num2
    num3=`expr $num1 + 200`
    echo $num3
    num4=`expr \( $num1 + 200 \) / 10 \* 2` 
    echo $num4
    exit 0

출력 화면

![Day%204/Untitled%201.png](images/Day%204/Untitled%201.png)

- **파라미터 변수**
    - $0, $1, $2 등의 형태
    - 실행하는 명령의 부분 하나를 변수로 지정

    #!bin/sh
    echo "실행파일 이름은 <$0>이다."
    echo "첫번째 파라미터는 <$1>이고, 두번째 파라미터는 <$2>이다."
    echo "전체 파라미터는 <$*>이다."
    exit 0

![Day%204/Untitled%202.png](images/Day%204/Untitled%202.png)

### 7.2.3 if문, case문

[조건]사이의 각 단어는 모두 **공백**이 있어야 한다!

- **기본 if문**

    if [ 조건 ]
    then
    	참일 경우 실행
    fi

    #!bin/sh
    if [ "woo" = "woo" ]           // 참
    then
    	echo "참입니다"              // 출력
    fi
    exit 0

- **if~else 문**
- **조건문에 들어가는 비교 연산자**

[문자열 비교 연산자](https://www.notion.so/e0c4655f176645989b6bd37c01fba47a)

[산술 비교 연산자](https://www.notion.so/fd0bdf8025cc4a97843806b459c01624)

    #!/bin/sh
    if [ 100 -eq 200 ]        // 숫자 100과 숫자 200은 다르므로, 거짓
    then
    	echo "100과 200은 같다"
    else
    	echo "100과 200은 다르다"
    fi
    exit 0

- **파일 관련 조건**

[파일 조건](https://www.notion.so/dd2dac9953964544b07d61fe5d058a00)

![Day%204/Untitled%203.png](images/Day%204/Untitled%203.png)

- **case ~ esac 문**

    // 예제 1 (case2.sh)
    
    #!/bin/sh
    case "$1" in
    	start)
    		echo "시작 ";; //break
    	stop)
    		echo "중지 ";;
    	restart)
    		echo "다시 시작";;
    	*)
    		echo "뭔지 모름";;
    
    esac
    exit 0
    

![Day%204/Untitled%204.png](images/Day%204/Untitled%204.png)

    // 예제 2 (case1.sh)
    #!/bin/sh


![Day%204/Untitled%205.png](images/Day%204/Untitled%205.png)

- **AND, OR 관계 연산자**
    - `-a` 또는 `&&` : AND 연산자
    - `-o` 또는 `||` : OR 연산자

    #!/bin/sh
    echo "보고 싶은 파일명을 입력"
    read fname
    if [ -f fname ] && [ -s $fname ] ; then
    	head -5 $fname
    else
    	echo "파일이 없거나, 크기가 0 입니다."
    fi
    exit 0

![Day%204/Untitled%206.png](images/Day%204/Untitled%206.png)

### 7.2.4 반복문

- **for ~ in 문**

    for 변수 in 값1 값2 값3 ... 
    do
    	반복할 문장
    done

- **while문**

    while [ 조건 ]
    do
    	반복할 문장
    done

- **until문**

    // [조건식]이 참일 때까지, 계속 반복
    until [ $i -gt 10]

- **break, continue, exit, return**
    - break : 반복문 종료
    - continue : 반복문의 조건식으로 돌아가게 함
    - exit : 프로그램을 완전히 종료

### 7.2.5 기타

- **사용자 정의 함수**

함수 이름( ) {      → 함수 정의
     내용
}

함수이름            → 함수 호출

- **함수의 파라미터**

함수 이름( ) {                                    → 함수 정의
   $1, $2 등을 사용
}
함수 이름 파라미터1 파라미터2      → 함수 호출

- **eval**
- 문자열을 명령문으로 인식하고 실행
- **export
- 외부 변수로 선언한다 (⇒ 다른 프로그램에서도 사용 가능)**
- **printf** 
- 형식을 지정하여 출력
⇒ 보안측면에서 **사용 지양**

    // printf.sh
    #!/bin/sh
    var1=100.5
    var2="재밌는 리눅스"
    //var2변수의 값 중간에 공백이 있어, 변수 이름을 ""로 묶어야 오류가 발생하지 않음
    printf "%5.2f \n\n \t %s \n" $var1 "$var2"
    exit 

- **set과 $(명령)**
- $(명령) : 리눅스 명령을 결과로 사용
- set : 결과를 파라미터로 사용할 때 사용

    // set.sh
    #!/bin/sh
    echo "오늘 날씨는 $(date) 입니다."
    set $(date)
    echo "오늘은 $4 요일입니다."
    exit 0

![Day%204/Untitled%207.png](images/Day%204/Untitled%207.png)