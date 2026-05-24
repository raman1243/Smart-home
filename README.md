<!DOCTYPE html>
<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Smart Home Control</title>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
min-height:100vh;
background:linear-gradient(-45deg,#0f2027,#203a43,#2c5364,#000000);
background-size:400% 400%;
animation:bg 12s ease infinite;
display:flex;
justify-content:center;
align-items:center;
padding:20px;
color:white;
overflow-x:hidden;
}

@keyframes bg{
0%{background-position:0% 50%;}
50%{background-position:100% 50%;}
100%{background-position:0% 50%;}
}

.container{
width:100%;
max-width:1100px;
}

.glass{
background:rgba(255,255,255,0.08);
backdrop-filter:blur(15px);
border-radius:25px;
box-shadow:0 0 25px rgba(0,0,0,0.4);
border:1px solid rgba(255,255,255,0.1);
}

h1{
text-align:center;
font-size:40px;
margin-bottom:35px;
text-shadow:0 0 20px cyan;
}

/* LOGIN */

#loginBox{
width:350px;
margin:auto;
padding:40px 30px;
text-align:center;
}

input{
width:100%;
padding:15px;
margin-top:15px;
border:none;
outline:none;
border-radius:50px;
font-size:16px;
background:rgba(255,255,255,0.15);
color:white;
text-align:center;
}

input::placeholder{
color:#ddd;
}

button{
cursor:pointer;
border:none;
transition:0.4s;
}

.loginBtn{
width:100%;
padding:15px;
margin-top:20px;
border-radius:50px;
font-size:18px;
font-weight:bold;
background:linear-gradient(45deg,#00c6ff,#0072ff);
color:white;
}

.loginBtn:hover{
transform:scale(1.05);
box-shadow:0 0 20px cyan;
}

/* DASHBOARD */

.dashboard{
display:none;
}

.cards{
display:flex;
justify-content:center;
gap:25px;
flex-wrap:wrap;
}

.card{
width:320px;
padding:25px;
text-align:center;
transition:0.4s;
}

.card:hover{
transform:translateY(-10px);
box-shadow:0 0 25px rgba(0,255,255,0.3);
}

.card h2{
font-size:30px;
margin-bottom:20px;
}

.status{
font-size:30px;
font-weight:bold;
margin-bottom:20px;
}

.on{
color:#00ff99;
text-shadow:0 0 15px #00ff99;
}

.off{
color:#ff4444;
text-shadow:0 0 15px #ff4444;
}

.controlBtns{
display:flex;
gap:10px;
margin-top:15px;
}

.onBtn{
flex:1;
padding:14px;
border-radius:50px;
background:linear-gradient(45deg,#00b09b,#96c93d);
color:white;
font-size:18px;
font-weight:bold;
}

.offBtn{
flex:1;
padding:14px;
border-radius:50px;
background:linear-gradient(45deg,#ff416c,#ff4b2b);
color:white;
font-size:18px;
font-weight:bold;
}

.onBtn:hover,
.offBtn:hover{
transform:scale(1.05);
}

.countdown{
margin-top:20px;
font-size:24px;
font-weight:bold;
color:#00e5ff;
text-shadow:0 0 10px cyan;
}

.bottomBtns{
margin-top:35px;
display:flex;
justify-content:center;
gap:20px;
flex-wrap:wrap;
}

.bottomBtns button{
padding:15px 35px;
border-radius:50px;
font-size:18px;
font-weight:bold;
color:white;
}

#allOff{
background:linear-gradient(45deg,#ff416c,#ff4b2b);
}

#logoutBtn{
background:linear-gradient(45deg,#434343,#000000);
}

@media(max-width:768px){

.card{
width:100%;
}

h1{
font-size:30px;
}

#loginBox{
width:100%;
}

}

</style>

</head>

<body>

<div class="container">

<h1>SMART HOME CONTROL</h1>

<!-- LOGIN -->

<div id="loginBox" class="glass">

<input type="email" id="email" placeholder="Enter Email">

<input type="password" id="password" placeholder="Enter Password">

<button class="loginBtn" id="loginBtn">
LOGIN
</button>

</div>

<!-- DASHBOARD -->

<div class="dashboard" id="dashboard">

<div class="cards">

<!-- RELAY 1 -->

<div class="card glass">

<h2>Relay 1</h2>

<div class="status off" id="status1">
OFF
</div>

<div class="controlBtns">

<button class="onBtn"
onclick="setRelay(1,true)">
ON
</button>

<button class="offBtn"
onclick="setRelay(1,false)">
OFF
</button>

</div>

<input type="number"
id="timer1"
placeholder="Enter Seconds">

<div class="controlBtns">

<button class="onBtn"
onclick="startTimer(1,'on')">
Timer ON
</button>

<button class="offBtn"
onclick="startTimer(1,'off')">
Timer OFF
</button>

</div>

<div class="countdown"
id="count1">
00
</div>

</div>

<!-- RELAY 2 -->

<div class="card glass">

<h2>Relay 2</h2>

<div class="status off" id="status2">
OFF
</div>

<div class="controlBtns">

<button class="onBtn"
onclick="setRelay(2,true)">
ON
</button>

<button class="offBtn"
onclick="setRelay(2,false)">
OFF
</button>

</div>

<input type="number"
id="timer2"
placeholder="Enter Seconds">

<div class="controlBtns">

<button class="onBtn"
onclick="startTimer(2,'on')">
Timer ON
</button>

<button class="offBtn"
onclick="startTimer(2,'off')">
Timer OFF
</button>

</div>

<div class="countdown"
id="count2">
00
</div>

</div>

</div>

<div class="bottomBtns">

<button id="allOff">
ALL OFF
</button>

<button id="logoutBtn">
LOGOUT
</button>

</div>

</div>

</div>

<!-- FIREBASE -->

<script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-app-compat.js"></script>

<script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-auth-compat.js"></script>

<script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-database-compat.js"></script>

<script>

// ================= FIREBASE CONFIG =================

const firebaseConfig = {
  apiKey: "AIzaSyCVERmq3MY_lhAPDGukVo09bPXYL0Dkbb0",
  authDomain: "my-smart-home-d3b00.firebaseapp.com",
  databaseURL: "https://my-smart-home-d3b00-default-rtdb.firebaseio.com",
  projectId: "my-smart-home-d3b00",
  storageBucket: "my-smart-home-d3b00.firebasestorage.app",
  messagingSenderId: "680614958456",
  appId: "1:680614958456:web:2c78bc08e7f0b2a136e48d"
};

firebase.initializeApp(firebaseConfig);

// ================= ELEMENTS =================

const loginBtn = document.getElementById("loginBtn");

const loginBox = document.getElementById("loginBox");

const dashboard = document.getElementById("dashboard");

// ================= LOGIN =================

loginBtn.onclick = ()=>{

const email = document.getElementById("email").value;

const password = document.getElementById("password").value;

firebase.auth().signInWithEmailAndPassword(email,password)

.then(()=>{

loginBox.style.display="none";

dashboard.style.display="block";

startControl();

})

.catch((error)=>{

alert(error.message);

});

};

// ================= START CONTROL =================

function startControl(){

const db = firebase.database();

const relay1 = db.ref("/relay1");

const relay2 = db.ref("/relay2");

// RELAY 1 STATUS

relay1.on("value",(snapshot)=>{

const state = snapshot.val();

const status = document.getElementById("status1");

status.innerHTML = state ? "ON" : "OFF";

status.className = state ? "status on" : "status off";

});

// RELAY 2 STATUS

relay2.on("value",(snapshot)=>{

const state = snapshot.val();

const status = document.getElementById("status2");

status.innerHTML = state ? "ON" : "OFF";

status.className = state ? "status on" : "status off";

});

// ALL OFF

document.getElementById("allOff").onclick = ()=>{

relay1.set(false);

relay2.set(false);

};

}

// ================= MANUAL CONTROL =================

function setRelay(relay,state){

firebase.database().ref("/relay"+relay).set(state);

}

// ================= TIMER =================

function startTimer(relay,action){

let seconds = parseInt(
document.getElementById("timer"+relay).value
);

if(seconds <= 0 || isNaN(seconds)){

alert("Enter valid seconds");

return;

}

const relayRef =
firebase.database().ref("/relay"+relay);

const countdown =
document.getElementById("count"+relay);

countdown.innerHTML =
(action === "on" ? "ON " : "OFF ")
+ seconds + "s";

let timer = setInterval(()=>{

seconds--;

countdown.innerHTML =
(action === "on" ? "ON " : "OFF ")
+ seconds + "s";

if(seconds <= 0){

clearInterval(timer);

if(action === "on"){

relayRef.set(true);

countdown.innerHTML = "RELAY ON";

}
else{

relayRef.set(false);

countdown.innerHTML = "RELAY OFF";

}

}

},1000);

}

// ================= LOGOUT =================

document.getElementById("logoutBtn").onclick = ()=>{

firebase.auth().signOut()

.then(()=>{

dashboard.style.display="none";

loginBox.style.display="block";

});

};

</script>

</body>

</html>
