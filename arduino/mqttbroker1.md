# MQTT 브로커에서 수신된 데이터를 DB에 저장하고 소켓통신을 통해 HTML 페이지에 데이터를 업데이트하는 코드 (Node.js)  


/**
 * Module dependencies.
 */

 var app = require('../app');
 var debug = require('debug')('iotserver:server');
 var http = require('http');
 
 /**
  * Get port from environment and store in Express.
  */
 
 var port = normalizePort(process.env.PORT || '3000');
 app.set('port', port);
 
 /**
  * Create HTTP server.
  */
 
 var server = http.createServer(app);
 
 // Connect Mongo DB 
 var mongoDB = require("mongodb").MongoClient;
 var url = "mongodb://127.0.0.1:27017/Priends";
 var dbObj = null;
 mongoDB.connect(url, function(err, db){
   dbObj = db;
   console.log("DB connect");
 });
 
 /**
  * MQTT subscriber (MQTT Server connection & Read resource data)
  */
 var mqtt = require("mqtt");
 var client = mqtt.connect("mqtt://127.0.0.1")
 
 // 접속에 성공하면, 2가지 토픽을 구독.
 client.on("connect", function(){
   client.subscribe("TMP");
   console.log("Subscribing TMP");
   client.subscribe("BPM");
   console.log("Subscribing BPM");
 })

// MQTT 응답 메세지 수신시 동작
 client.on("message", function(topic, message){
   console.log(topic+ ": " + message.toString()); // 수신한 메세지 Topic 출력
   var obj = JSON.parse(message); // 수신한 메세지의 데이터를 obj 저장
   obj.create_at = new Date(); // 현재 날짜 데이터를 obj에 추가함.
   console.log(obj);
 
   // send the received data to MongoDB
   // 수신한 메세지를 Mongo DB에 저장
   if (topic == "TMP"){ // 만약 토픽이 온도라면,
     var TMP  = dbObj.collection("TMP"); // TMP라는 이름을 갖은 collection 선택
     TMP.save(obj, function(err, result){ // collection에 온도 데이터 저장
       if (err){
         console.log(err);
       }else{
         console.log(JSON.stringify(result));
       }    
     });  
   }else if (topic == "BPM"){ // 만약 토픽이 심박이라면,
     var BPM  = dbObj.collection("BPM"); // BPM라는 이름을 갖은 collection 선택
     BPM.save(obj, function(err, result){ // collection에 심박 데이터 저장
       if (err){
         console.log(err);
       }else{
         console.log(JSON.stringify(result));
       }    
     });
   }
 });
  
 // get data from MongDB and then send it to HTML page using socket
 // Mongo DB에서 최근 데이터 불러와서, HTML 페이지에 업데이트
 var io = require("socket.io")(server);
 io.on("connection", function(socket){
   socket.on("socket_evt_update", function(data){
     var TMP = dbObj.collection("TMP"); // TMP 라는 이름의 collection 선택
     var BPM = dbObj.collection("BPM"); // BPM 라는 이름의 collection 선택
     // 온도 데이터
     TMP.find({}).sort({_id:-1}).limit(1).toArray(function(err, results){
       // collection에서 가장 최근 데이터 정렬-> 하나의 데이터만 불러옴 -> 배열로 만듬
       if(!err){
         console.log(results[0]);
         socket.emit("socket_up_TMP", JSON.stringify(results[0]));
       }
     });
     // 심박 데이터
     BPM.find({}).sort({_id:-1}).limit(1).toArray(function(err, results){
       // collection에서 가장 최근 데이터 정렬-> 하나의 데이터만 불러옴 -> 배열로 만듬
       if(!err){
         console.log(results[0]);
         socket.emit("socket_up_BPM", JSON.stringify(results[0]));
       }
     });
   });
 });
 
 
 /**
  * Listen on provided port, on all network interfaces.
  */
 server.listen(port);
 server.on('error', onError);
 server.on('listening', onListening);
 
 /**
  * Normalize a port into a number, string, or false.
  */
 function normalizePort(val) {
   var port = parseInt(val, 10);
   if (isNaN(port)) {
     // named pipe
     return val;
   }
   if (port >= 0) {
     // port number
     return port;
   }
   return false;
 }
 
 /**
  * Event listener for HTTP server "error" event.
  */
 
 function onError(error) {
   if (error.syscall !== 'listen') {
     throw error;
   }
 
   var bind = typeof port === 'string'
     ? 'Pipe ' + port
     : 'Port ' + port;
 
   // handle specific listen errors with friendly messages
   switch (error.code) {
     case 'EACCES':
       console.error(bind + ' requires elevated privileges');
       process.exit(1);
       break;
     case 'EADDRINUSE':
       console.error(bind + ' is already in use');
       process.exit(1);
       break;
     default:
       throw error;
   }
 }
 
 /**
  * Event listener for HTTP server "listening" event.
  */
 
 function onListening() {
   var addr = server.address();
   var bind = typeof addr === 'string'
     ? 'pipe ' + addr
     : 'port ' + addr.port;
   debug('Listening on ' + bind);
 }
 
