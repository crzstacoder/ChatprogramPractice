# ChatprogramPractice

## where I studied in...
https://geundung.dev/62?category=719250

-node.js express 프레임워크로 실시간 채팅서버 만들기  
+ socket.io 라이브러리 사용  
  
만들었던 다른 서버와 달리, npm init으로 pakage.json 폴더를 만든 후  
express를 설치했다.  
  
제일 먼저 socket.io를 npm install 해주었다.  
app.js 파일을 따로 생성 해 주었다.  
  
  
app.js파일-------------------------------------------------------------------  
  
  
express모듈, socket.io모듈을 불러왔고,  
내장 모듈으로는, http, fs를 불러왔다.  
여기서 fs모듈은 파일을 다루는 함수들이 있다.  
http 모듈의 creatServer함수로 서버를 만들었다.  
생성된 서버를 socket.io에 바인딩 하는데,  
이때 바인딩은 실제 socket.io가 위치한 메모리를 연결해주는 것이라고 보면 된다.  
  
Get 방식으로, 상세 uri를 /으로, 경로에 접속하는 로직을 만들어보자  
  
fs모듈의 readFile함수(request, response객체를 받음)를 이용해 index.html를 찾고,  
---> request에는 요청한 클라이언트에서 전달된 데이터와 정보들이 있고,  
---> response에는 응답을 하기 위한 정보가 있다.  
에러 시 '에러'응답을 보내주게 설정,  
에러가 아닐 경우에는 writeHead함수를 사용하여 헤더에 HTML이라는 것을 표시,  
write함수로 HTML데이터를 전송하고  
end함수를 이용해서 전송을 마친다.  
  
socket의 on함수는 콜백함수 ( on은 수신, emit은 전송이다)  
io.sockets은 접속되는 모든 소켓들을 의미한다.  
  
io.sockets.on('connection', function(socket) {  
})  
위 코드는 io.sockets의 범위에서 connection이라는 이벤트가 발생할 경우에 on(콜백함수)가 실행된다.  
  
따라서 connect이벤드 발생과 동시에 콜백함수로 전달되는 소켓은 접속된 소켓들이다.  
  
다시 newUser이벤트 발생 시 이름을 묻고, 그 이름을 소켓에 저장한다.  
그 후 io.sockets(모든 소켓)에 특정유저 접속을 emit(전송)한다.  
  
전송한 메시지를 받아야 하므로, message이벤트가 발생했을 때, 콜백해준다.  
누가 데이터를 보냈는지 이름을 추가하고, data를 출력한다.  
socket모듈의 broadcast 함수를 이용해서 자신을 제외한 모두에게 데이터를 전송한다.  
( io.sockets.emit() -> 본인 포함 모든 유저에게 데이터 전송 )  
( io.broadcast.emit() -> 본인 제외 모든 유저에게 데이터 전송 )  
  
접속 종료를 뜻하는 disconnect이벤트가 발생 시, 서버에서 누가 나갔는지 출력해준다.  
이도 마찬가지로 broadcast를 활용하여 본인을 제외한 모든 사람들에게 나간다는 메시지를 전송한다.  
  
아래 listen은 서버를 특정 포트로 열어준다.  