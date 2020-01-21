# 노드의 기능 - 이벤트, 예외처리

### 이벤트

- `on('data', 콜백)` 또는 `on('end', 콜백)`
**⇒ data와 end 이벤트가 발생 할 때, 콜백함수를 호출**
- 활용
    1. `events` 모듈로 객체(myEvent) 를 생성

            const EventEmitter = require('events');
            
            const myEvent = new EventEmitter();

    2. 생성한 객체의 메서드를 이용

        ```javascript
        // event1 이벤트 생성 (메서드: addListener)
        myEvent.addListener('event1', ()=>{});
        
        // event2 이벤트 생성 (메서드: on)
    myEvent.on('event2', ()=>{});
        ```
        
        
    
- 관련 메서드
    1. `on(이벤트명, 콜백)` : 이벤트 이름과 이벤트 발생 시의 콜백을 연결 

        이벤트 리스닝 : 연결하는 동작

    2. `addListener(이벤트명, 콜백)` : `on`과 동일한 기능
    3. `emit(이벤트명)` : 이벤트를 호출
    4. `once(이벤트명, 콜백)` : 한 번만 실행되는 이벤트 
    5. `removeAllListeners(이벤트명)` : 이벤트에 연결된 모든 이벤트 리스너를 제거
    6. `removeListener(이벤트명, 리스너)` : 이벤트에 연결된 리스너를 하나씩 제거
    7. `off(이벤트명, 콜백)` : removeListener와 동일한 기능
    8. `listenerCount(이벤트명)` : 현재 연결된 리스너의 수

### 예외 처리 (error)

- `try catch`문으로 예외 처리를 위한 코드 작성

(예시) 

```javascript
setInerval(()=>{
	console.log('시작');
	try {
			throw new Error('서버 고장');   // 에러 발생 코드
	} catch(err) {
			console.error(err);             // 프로세스를 죽지 않게 하는 구문
	}
} , 1000);
```