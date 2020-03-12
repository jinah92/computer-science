1. index.html

   ```javascript
   <!-- 1. Javascript 방식 -->
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <meta http-equiv="X-UA-Compatible" content="ie=edge">
       <title>JS Page</title>
       <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
       <link rel="stylesheet" href="./css/main.css">
       <script src="./client.js"></script>
   <body>
       <script>
           function sendMsg_ok(){         
                   alert('환영합니다! (정적 페이지 이동)');
           }
           function sendMsg_deny(){
               alert('다시 입력해주세요');
           }
       </script>
   
       <div id="container">
           <h2>우리의 재밌는 놀이</h2>
           <form>
               <input type="text" id="name" value="my name" /><br><br>
               <input type="password" id="pw" value="my password" /><br>
               <a id="ok_btn" onclick="sendMsg_ok()" href="html/login.html"><input type="button" class="btn" value="정적 페이지 이동"/></a>
               <input type="button" class="btn" id="ok_btn_jqeury" value="JQuery">
               <input type="button" class="btn" id="ok_btn_react" value="React">
               <input type="button" class="btn" id="deny_btn" onclick="sendMsg_deny()" value="취소" />
           </form>
               
           <div id="content_message" style="display: none;">
               <img id="image" src="img/puppy.jpg" alt="puppy" width="100px" height="150px">
               <div id="login_info">
                       <p id="login_user"></p>
                       <p id="login_pw"></p>
               </div>
           </div>
       
           
       </div>
   </body>
   </html>
   ```

   

2. client.js

   ```javascript
   $(document).ready(function(){
       $('#ok_btn_jqeury').click(function(){
           const name = $('#name').val();
           const pw = $('#pw').val();
           
           if(name === "my name"){
               alert("다시 입력하세요");
           } else {
               alert('동적 화면이동! (JQuery)');
               $('#login_user').text(name);
               $('#login_pw').text(pw);
               $('#content_message').show();
           }
           
       });
   });
   ```

   

3. index.jsx

   ```jsx
   class Show_userInfo extends React.Component {
       render() {
           return (
               <h1>{this.props.name}</h1>
           );
       }
   }
   
   class Input_userInfo extends React.Component {
       username = {message: ""};
       inputName = () => {
           this.setState({
               message: "입력완료!"
           });
       }
       render() {
           return (
               <div>
                   <Show_userInfo name={this.state.message} />
                   <input type="button" onClick={this.inputName} value="input name"/>
               </div>
           );
       }
   }
   
   ReactDOM.render(
       <Input_userInfo />, document.body
   );
   ```

   