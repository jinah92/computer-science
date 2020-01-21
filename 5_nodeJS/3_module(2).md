# 노드의 기능 - (내장 모듈)

## 노드 내장 모듈

---

- 노드에서 제공하는 모듈을 사용하여, 운영체제 정보에 접근하거나 클라이언트가 요청한 주소에 대한 정보 등을 가져 올 수 있음.

### 1. os

- 운영체제 정보를 가져옴

컴퓨터 내부 자원에 빈번하게 접근하는 경우에 사용

- `os.arch()` : process.arch와 동일
- `os.platform()` : process.paltform과 동일
- `os.type()` : 운영체제의 종료
- `os.uptime()` : 운영체제 부팅 이후 흐른 시간(초)   ※ `process.uptime()` : 노드의 실행시간
- `os.hostname()` : 컴퓨터의 이름
- `os.release()` : 운영체제의 버전
- `os.homedir()` : 홈 디렉터리 경로
- `os.tmpdir()` : 임시파일 저장 경로
- `os.cpus()` : 컴퓨터의 코어 정보
- `os.freemem()` : 사용 가능한 메모리(RAM)
- `os.totalmem()` : 전체 메모리 용량

### 2. path

폴더와 파일의 경로를 쉽게 조작하도록 함

- `path.sep` : 경로 구분자 (윈도우 ⇒  \)
- `path.delimiter` : 환경 변수의 구분자 (윈도우 ⇒ ; )
- `path.dirname(경로)` : 파일이 위치한 폴더 경로
- `path.extname(경로)` : 파일의 확장자
- `path.basename(경로, 확장자)` : 파일의 이름(확장자 포함)을 출력
- `path.parse(경로)` : 파일 경로를 분리 (root, dir, base, ext, name)
- `path.format(객체)` : path.parse()한 객체를 파일 경로로 합침
- `path.normalize(경로)` : 정상적인 경로로 변환
- `path.isAbsolute(경로)` : 절대 경로 (⇒ true) 또는 상대 경로(⇒ false)를 구분
- `path.relative(기준경로, 비교경로)` : 기준 경로에서 비교 경로로 가는 방법
- `path.join(경로, .. )` : 여러 인자를 하나의 경로로 합침 (상대경로로 처리)
- `path.resolve(경로, ..)` : 여러 인자를 하나의 경로로 합침 (/를 만나면 앞의 경로를 무시 = 절대경로 처리)

### 3. url

- 인터넷 주소를 쉽게 조작하도록 도와주는 기능 (2가지 처리방식: WHATWG, 기존 url)
- `url.parse(주소)` : 주소를 분해
- `url.format(객체)` : 분해되었던 url 객체를 다시 원래 상태로 조립

### 4. querystring

- WHATWG 방식의 url 대신, 기존 노드의 url을 사용할 때 search 부분을 사용하기 쉽게 객체로 만드는 모듈
- `querystring.parse(쿼리)` : url의 query 부분을 자바스크립트 객체로 분해
- `querystring.stringify(객체)` : 분해된 query 객체를 문자열로 다시 조립