# 노드의 기능 - 1

### 모듈

---

- 노드는 코드를 모듈로 만들 수 있다 (브라우저의 자바스크립트와 다른 점)

- 파일 하나 = 모듈 하나

- 예제

- var.js

    ```javascript
    const odd = '홀수';
    const even = '짝수';
    
    module.exports = {
    	odd, 
    	even,
    };
    ```

    

- func.js

    ```javascript
    const { odd, even } = require('./var');  // 같은 위치의 var.js 모듈을 불러옴 (reauire)
    
    function checkOddOrEven(num) {
        if (num % 2) {  // 홀수 (true === 1)
            return odd;
        } else {        // 짝수 (false === 0)
            return even;
        }
    }
    
    module.exports = checkOddOrEven;         // 해당 파일을 모듈로서 사용 (module.exorts)  
    ```
    
    

### 노드 내장 객체

---

### 1. global

- 브라우저의 window와 같은 전역객체  (⇒ 모든 파일에서 접근 가능)

- globalA.js

    ```javascript
    module.exports = () => global.message;
    ```

    

- globalB.js

    ```javascript
    const A = require('./globalA');
    
    global.message = '안녕하세요';
    console.log(A());
    ```
    
    
    
    - globalB에서 넣은 global.message 값을 globalA에서도 접근 가능
    
    - 모듈 : 제공받는 모듈은 경로를 지정하지 않음
    
      

### 2. console

- global 객체 안에 있는 객체

- 디버깅 목적

- 예제

    ```javascript
    const string = 'abc';
    const number = 1;
    const boolean = true;
    const obj = {
        outside: {
            inside: {
                key: 'value',
            },
        },
    };
    console.time('전체 시간');
    console.log('평범한 로그입니다. 쉼표를 구분해 여러 값을 찍을 수 있습니다.');
    console.log(string, number, boolean);
    console.error('에러 메시지는 console.error 에 담아주세요');
    
    console.dir(obj, {colors: false, depth: 2});
    console.dir(obj, {colors: true, depth: 1});
    
    console.time('시간 측정');
    for (let i = 0; i<100000; i++){
        continue;
    }
    console.timeEnd('시간 측정');
    
    function b() {
        console.trace('에러 위치 추적');
    }
    function a() {
        b();
    }
    a();
    
    console.timeEnd('전체 시간');
    ```
    
    

- `console.time(레이블)` : `console.timeEnd(레이블)`과 대응하여 같은 레이블을 가진 time과 timeEnd 사이의 시간을 측정

- `console.log(내용)` : 로그를 콘솔에 표현

- `console.err(에러 내용)` : 에러를 콘솔에 표현

- `console.dir(객체, 옵션)` : 객체를 콘솔에 표현 (옵션: colors = true ⇒ 콘솔에 색 추가)

- `console.trace(레이블)` : 에러가 어디서 발생했는지를 추적

  

### 3. 타이머

- global 객체 안에 있는 객체

    (콜백함수 실행)
    `setTimeout(콜백 함수, 밀리초)` : 밀리초 이후에 콜백 함수를 실행

    `setInterval(콜백 함수, 밀리초)` : 주어진 밀리초 마다 콜백 함수를 반복 실행

    `setImediate(콜백 함수)` : 콜백 함수를 즉시 실행

    (타이머 종료)
    `clear Timeout(아이디)` : setTimeoust 취소

    `clear Interval(아이디)`  : setInterval 취소

    `clear Imediate(아이디)` : setImediate 취소

### 4. __filename, __dirname

- 현재 파일명과 파일 경로를 확인

### 5. module, exports

- 모듈 생성 : **module객체( `module.exports`)** 또는 **export 객체** 이용
    1. module.exports : 한 번에 대입
    2. exports : 각 변수를 객체에 대입

exports 객체
 - 반드시 속성명과 속성값을 함께 대입해야 함
 - 오직 객체만 대입 가능 (함수 대입 불가)

module 객체
 - 객체와 함수 모두 대입 가능

### 6. process

- 현재 실행되고 있는 노드 프로세스에 대한 정보

- `process.env` : 시스템의 환경 변수 (비밀번호 등의 중요정보는 `process.env`의 속성으로 대체)

    const secretId = process.env.SECRET_ID;
    const secreteCode = process.env.SECRET_CODE;

- `process.nextTick(콜백)` : 이벤트 루프가 다른 콜백 함수들보다 `nextTick`의 콜백 함수를 우선 처리 (`Promise`, `nextTick` ⇒ 다른 콜백함수들보다 우선됨)

    ```javascript
    setImmediate(() => {
    	console.log('immediate');
    });
    process.nextTick(()=>{
    	console.log('next Tick');
    });
    setTimeout(()=>{
    	console.log('timeout');
    }, 0);
    Promise.resolve().then(() => console.log('promise'));
    ```

    

- `process.exit()` : 실행 중인 노드 프로세스를 종료

- (예제) `setInterval`로 반복되고 있는 코드를 종료

    ```javascript
    let i = 1;
    setInterval(()=>{
    	if (i===5) {
    		console.log('종료!');
    		process.exit();
    	}
    	console.log(i);
    	i += 1;
    }, 1000);
    ```