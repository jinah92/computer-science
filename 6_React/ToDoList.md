# ToDoList

### 1. create-app-react [프로젝트명]

### 2. public 폴더와 src 폴더 내용 지우기

### 3. (public 폴더) index.html 을 아래와 같이 작성하기

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>List Practice</title>
</head>
<body>
    <div id="container"></div>
</body>
</html>
```

### 4. (src 폴더) index.jsx 만들기 (전체 화면이 뿌려질 Component)

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
    <ToDoList />,   //커스텀 태그 (리스트 항목 뷰)
    document.querySelector('#container')
);

export default ToDoList;
```

### 4-1. 상단에 참조할 커스텀 태크(ToDoList) 파일 추가

```javascript
import ToDoList from './ToDoList';
```

### 5. (src 폴더) ToDoList.jsx 만들기 (부모 Component, 전체 리스트)

```javascript
import React, {Component} from 'react';

class ToDoList extends Component {
    state={
        items: []  //모든 리스트를 관리하는 배열
    }
    render(){
        return(
            <div className="todoListMain">
                <div className="header">
                    <input></input> {/*데이터 입력 부분*/}
                    <button>add</button>  {/*입력 버튼*/}
                </div>
                <ToDoItems /> {/*ToDoItems 커스텀 태그*/}
            </div>
        );
    }
}

export default ToDoList;
```

### 6. 리스트 추가 기능(addItem)

### 6-1.  ToDoList.jsx (부모 Component)

---

```javascript
addItem=()=>{
        this.state.items.unshift({       // unshift: 최신 입력을 items 배열의 최초 인덱스 요소에 저장
            text: this.inputElement.value,   //입력 값
            key: Date.now()                  //리스트 구분 값(입력된 현재 시간)
        });
        this.setState({
            items: this.state.items      // 추가된 리스트를 state에 갱신
        });
        console.log(this.state.items);
        this.inputElement.value = "";    
        this.inputElement.focus();      
    }

 render(){
        return(
            <div className="todoListMain">
                <div className="header">
                    <input ref={ref=()=>this.inputElement=ref}></input> {/*데이터 입력 부분*/}
                    <button onClick={this.addItem}>add</button>  {/*입력 버튼*/}
```

### 6-2. ToDoItems (자식 Component, 각 리스트 항목)

---

```javascript
import React, {Component} from 'react';

class ToDoItems extends Component {
    render(){
        return(
            <ul className="theList">
                {myList}  {/*각 리스트 항목*/}
            </ul>
        );
    }
}

export default ToDoItems;
```

### 6-3. 부모 Component (ToDoList) 에서 items 속성 넘기기

---

items를 entries라는 이름으로 자식 Component(ToDoList)에게 넘기기

```javascript
<ToDoItems entries={this.state.items}/>
```

### 6-4. 자식 Component (ToDoItems)에서 받은 items 속성을 렌더링하기

---

```javascript
class ToDoItems extends Component {
    render(){
        const myList = this.props.entries.map((item)=>{   // map : 콜백함수에서 실행된 결과의 배열을 반환 
            return <li key={item.key}>{item.text}</li>
        });
        return(
            <ul className="theList">
                {myList}
            </ul>
        );
    }
}
```

### 7. 리스트 삭제 기능(deleteItem)

### 7-1.  ToDoList.jsx (부모 Component)

---

```javascript
deleteItem=(key)=>{
        alert("삭제 완료");
        const filteredItems = this.state.items.filter((item)=>{
            return item.key !== key     //filter: 삭제할 item을 제외한 리스트들을 리턴하여 변수에 저장(filteredItems)
        });
        this.setState({
            items: filteredItems        //items 갱신
        });            
    }
```

superDelete 속성이라는 이름으로 ToDoItems 태그에 deleteItem을 전달

```javascript
retner(){
	return(
		....
		<ToDoItems entries={this.state.items} superDelete={this.deleteItem}/> //superDelete 속성 추가
	);
}
```

### 7-2. ToDoItems (자식 Component)

---

subDelete 메소드를 추가 (부모에게 자신의 키를 전달하면서 자신을 알리는 기능)

```javascript
subDelete=(key)=>{
        this.props.superDelete(key);   //리스트 자신의 key를 superDelete 속성에 저장
    }
```