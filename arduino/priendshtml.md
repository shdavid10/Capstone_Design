# Priends.html

<!DOCTYPE html>
<html>
<head>
<title>Priends</title>
<link href = "style.css" type = "text/css" rel = "stylesheet">
<script src = "http://code.jquery.com/jquery-1.10.2.js"></script>
<style>
table {
    width: 90%;
    height: 70px;
}

.menu {
    border-top: 3px solid rgb(58, 59, 68);
    border-bottom: 3px solid rgb(58, 59, 68);
}

td {
    width : 25%;
    text-align : center;
}

.dropdown {
        position: relative;
        display: inline-block;
}

.dropbtn {
        background-color: #D5D5D5;
        color:  rgb(58, 59, 68);
        height: 40px;
        font-size: 16px;
        border: none;
        cursor: pointer;
}

.dropdown-content {
        display: none;
        position: absolute;
        background-color: #f9f9f9;
        min-width: 160px;
        box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
        z-index: 1;
}

.dropdown-content a {
        color: black;
        padding: 12px 16px;
        text-decoration: none;
        display: block;
}

.dropdown-content a:hover {
    background-color: #f1f1f1;
    font-weight: bold;  
}

.dropdown:hover .dropdown-content {
    display: block;
}

.dropdown:hover .dropbtn {
    font-size: 20px;
    font-weight: bold;
}

iframe {
    width : 90%;
    height : 100%;
    border: 0px; 
}

h1 a:link {
    text-decoration: none;
    color: black;
}

h1 a:visited {
    text-decoration: none;
    color: black;
}

h1 a:hover {
    text-decoration: none;
    color: black;
}

h1 a:active {
    text-decoration: none;
    color: black;
}
</style>
</head>
<body>

<h1 align = "center"><a  href = "Main.html" target="main"><img src="media/logo.jpg" width="200" height="70"></a></h1>
<table class="menu" align = "center">
<tr>
<td>
    <div class = "dropdown">
        <button class = "dropbtn"><strong>구역1</strong></button>
        <div class = "dropdown-content">
            <a href = "#">대형</a>
            <a href = "PetInfo.html" target="main">중형</a>
            <a href = "#">소형</a>
        </div>
    </div>
</td>
<td>
    <div class = "dropdown">
        <button class = "dropbtn"><strong>구역2</strong></button>
        <div class = "dropdown-content">
            <a href = "#">대형</a>
            <a href = "#">중형</a>
            <a href = "#">소형</a>
        </div>
    </div>
</td>
<td>
    <div class = "dropdown">
        <button class = "dropbtn"><strong>구역3</strong></button>
        <div class = "dropdown-content">
            <a href = "#">대형</a>
            <a href = "#">중형</a>
            <a href = "#">소형</a>
        </div>
    </div>
</td>
<td>
    <div class = "dropdown">
        <button class = "dropbtn"><strong>구역4</strong></button>
        <div class = "dropdown-content">
            <a href = "#">대형</a>
            <a href = "#">중형</a>
            <a href = "#">소형</a>
        </div>
    </div>
</td>
</tr>
</table>

<table>
<tr><td><div align = "right">접속자 : Priends 동물병원<a></a></div></td></tr>
</table>

<section align = "center">
<iframe src = "Main.html" name = "main" scrolling = "no"></iframe>
</section>
</body>
</html>
