# PetDetail.html 

<!DOCTYPE html>
<html>
<meta charset="UTF-8">
<head>
<script src="/socket.io/socket.io.js"></script>
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script>
    var socket = io.connect();
    var timer = null;
    $(document).ready(function(){
        socket.on("socket_up_TMP", function(data){
            data = JSON.parse(data);
            $(".mqttlist_TMP").html(data.TMP);
        });
        socket.on("socket_up_BPM", function(data){
            data = JSON.parse(data);
            $(".mqttlist_BPM").html(data.BPM);
        });
        if(timer==null){
            timer = window.setInterval("timer_1()", 3000);
        }
    });
    function timer_1(){
        socket.emit("socket_evt_update", JSON.stringify({}));
    }
</script>
<style>
table {
    width: 100%;
}

tr, td {
    text-align: center;
    width: 30%;
}

img {
    width: 400px;
    height: 400px;
    border-radius: 200px;
}

.txtleft {
    text-align: left;
    font-size: 35px;
}
</style>
</head>
<body>
<table>
    <tr>
        <td></td>
        <td><img src="media/gaul2.jpg"></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td class="txtleft">
            <strong>이름:</strong> 가을이<br>
            <strong>성별:</strong> 수컷<br>
            <strong>나이:</strong> 9<br>
            <strong>병명:</strong> 사랑스러움 과다<br>
            <strong> 체온:</strong> <a class="mqttlist_TMP"></a><br>
            <strong> 심박수:</strong> <a class="mqttlist_BPM"></a>
        </td>
        <td></td>
    </tr>
</table>
</body>
</html>

