# Node 개요

## 핵심 개념 이해

---

### Node JS: 크롬 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임

### 서버

---

- 네트워크를 통해 클라이언트에 정보나 서비스를 제공하는 컴퓨터 또는 프로그램

클라이언트: 요청을 보내는 주체
 (브라우저, 데스크톱 프로그램, 모바일 앱, 다른 서버에 요청을 보내는 서버 등)

### 자바스크립트 런타임

---

- 런타임 : 특정 언어로 만든 프로그램을 실행할 수 있는 환경

    노드는 자바스크립트 프로그램을 컴퓨터에서 실행할 수 있게 함

## 이벤트 기반 (event-driven)

---

- 이벤트가 발생할 때 미리 지정해둔 작업을 수행하는 방식 (클릭, 네트워크 요청 등)
- 이벤트 기반 시스템에서는 특정 이벤트 발생 시, 무엇을 할지 미리 등록해야 함
⇒ 이벤트 리스너에 콜백함수를 등록한다
- (ex) setTimeout 함수 ⇒ 비동기적 함수(메서드)

    노드는 자바스크립트 코드에서 맨 위부터 한 줄씩 실행한다

- 이벤트 루프 : 여러 이벤트가 동시에 발생했을 때, 어떤 순서로 콜백함수를 호출할 지를 판단 (노드가 종료될 때까지 이벤트 처리를 위한 작업을 반복)
- 테스크 큐 (=콜백 큐) : 이벤트 발생 후 호출되어야 할 콜백 함수들이 기다리는 공간
- 백그라운드 : 타이머나 I/O 작업 콜백, 이벤트 리스너들이 대기하는 곳

## 논블로킹 I/O

---

- 이전 작업이 완료될 때 까지 멈추지 않고, 다음 작업을 수행함 
※ 논블로킹 방식은 같은 작업을 더 짧은 시간동안 처리할 수 있음
- I/O (입출력) : 파일 시스템 접근, 네트워크 요청 등

    노드는 논블로킹 방식으로 동작

    `setTimeout(콜백, time)` : 코드를 논블로킹으로 만들기 위한 방법

## 싱글 스레드

---

노드는 싱글 스레드 + 논블로킹 모델을 채택함

- 싱글스레드, 멀티스레드
- 프로세스 vs 스레드
    1. 프로세스: 운영체제에서 할당하는 작업의 단위 (프로세스 간에는 메모리 등의 자원공유 X) 
    2. 스레드: 프로세스 내 실행되는 흐름의 단위 (같은 메모리에 접근 가능)
- 노드로 서버를 쓸 수 있다. (서버를 위해 노드를 쓰는 것이 아님!)