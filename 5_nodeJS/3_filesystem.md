# 노드의 기능 - (파일시스템 접근, 동기/비동기 메서드, 버퍼와 스트림)

Created By: 진아 최
Last Edited: Jan 21, 2020 7:52 PM

### `fs` 모듈 : 파일 시스템에 접근

1) 파일 읽기

    const fs = require('fs');
    
    fs.readFile('./readme.txt', (err, buffer)=>{
    	if(err) {
    		throw err;
    	}
    	console.log(buffer);
    	console.log(buffer.toString());
    });

`readFile`의 결과물 ⇒ '버퍼' 형식으로 제공 ⇒ toString()으로 문자열 변환 필요

2) 파일 쓰기 

    const fs = require('fs');
    
    fs.writeFile('./writeme.txt', '글이 입력', (err)=>{
    	if(err) {
    		throw err;
    	}
    	fs.readFile('./writeme.txt', (err, data)=>{
    		if(err) {
    			throw err;
    		}	
    		console.log(data.toString());
    	});
    });

### 동기 메서드, 비동기 메서드

노드는 대부분의 메서드를 **비동기 방식**으로 처리

호출하는 메서드의 처리 방식(동기, 비동기)에 따라 블로킹, 논블로킹을 결졍
 - 동기 메서드 호출 ⇒ 프로세스는 블로킹
 - 비동기 메서드 호출 ⇒ 프로세스는 논-블로킹

- `readFile` (비동기 메서드) , `readFileSync` (동기 메서드)

### 버퍼, 스트림

- 파일을 읽거나 쓰는 방식에는 크게 2가지 방식(버퍼, 스트림)이 있음

    노드는 파일을 읽을 때,
    1) 메모리에 파일 크기만큼 공간을 마련 후
    2) 파일 데이터를 메모리에 저장한 뒤 사용자가 조작할 수 있게 함

    **⇒ 버퍼 : 메모리에 저장된 데이터**

- Buffer : 버퍼를 직접 다루는 클래스
    - `from(문자열)` : 문자열을 버퍼로 변환
    - `toString(버퍼)` : 버퍼를 문자열로 변환
    - `concat(배열)` : 배열안에 든 버퍼들을 하나로 합침
    - `alloc(바이트)` : 바이트 크기 만큼의 빈 버퍼를 생성

**스트림 : 기존의 버퍼를 여러번 나눠서 보내는 방식**

- 파일 읽기
    1. 읽기 스트림을 생성 

            const readStream = createReadStream(파일경로, {highWaterMark: 버퍼의 크기});
            const data = [];

    2. 이벤트 리스터를 붙임

            readStream.on('data', (chunk)=>{
            
            // data 이벤트 (정의된 버퍼크기만큼 데이터를 읽음)
            	data.push(chunk);
            	console.log('data :', chunk, chunk.length);
            });
            
            // end 이벤트 (파일을 다 읽으면, concat으로 읽은 data를 문자열로 합침)
            readStream.on('end', ()=>{
            	console.log('end: ', Buffer.concat(data).toString());
            });
            
            // error 처리
            readStream.on('error', (err)=>{
            	console.log('error :', err);
            });

pipe : 스트림 사이에 데이터를 교환을 위해 연결하는 것