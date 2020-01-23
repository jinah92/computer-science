# Express 웹서버

### Express 개요

---

- npm에서는 서버 제작 시 불편함을 해소하고, 편의 기능을 추가한 웹 서버 프레임워크
- **Express-generator** : Express에 필요한 `package.json`을 만들고, 기본 폴더 구조를 만들어주는 패키지

### Express-generator 설치

---

1. 전역 설치 `npm i -g express-generator`
2. 새 Express 프로젝트 생성 `express [PROJECT_NAME] —view = [VIEW_NAME]`

- public 하위 폴더 (정적 문서)

- app.js (서블릿 객체 = 웹 컴포넌트 = 서비스 컴포넌트)
- views 하위 폴더 (jsp 파일)

### nodemon 패키지 설치

---

- 서버를 재가동하지 않아도 새로 작성된 서버 side 코드를 반영
1. 패키지 설치 `npm i -g nodemon`
2. package.json 파일에 start 속성 넣기 `start : nodemon [SERVICE_COMPONENT_NAME]`

### 웹 서버 만들기 순서

1. 프로젝트 폴더 생성
2. 생성한 프로젝트 폴더 경로 이동 (cmd)
3. npm 초기 관리를 선언 `npm init` 
4. express 패키지 설치 `npm i express`
5. package.json에 `start` 속성 부여
6. app.js 작성
7. `require('express')` ... `listen(port);`
8. public 폴더에 정적문서 생성 (html, css, js 등)
9. [http://localhost:port](http://localhost:port) 확인
10. client.js 작성
11. `$.post(url, send_param, callback);` 
12. app.js에서 post 요청 작업 처리 `app.post('/url', callback);`
13. app.js에서 `app.use(express.json());` 설정