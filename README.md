<!DOCTYPE html>
<html lang="lv">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Skolas Ziņu Centrs</title>
<style>
body {
  margin: 0;
  padding: 0;
  background: #e0e0e0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif;
}

/* ===== iPhone rāmja stils ===== */
.iphone-frame {
  width: 390px;
  height: 844px;
  border: 16px solid #222;
  border-radius: 60px;
  position: relative;
  background: #fff;
  box-shadow: 0 20px 50px rgba(0,0,0,0.4);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* Kamera augšā */
.camera-notch {
  width: 150px;
  height: 30px;
  background: #000;
  border-radius: 0 0 20px 20px;
  position: absolute;
  top: 0;
  left: calc(50% - 75px);
  z-index: 2;
}

/* Statusa josla */
.status-bar {
  height: 40px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 25px; /* baterija iekšā no malas */
  font-size: 14px;
  position: relative;
  z-index: 3;
}

/* Laiks */
.status-bar .time {
  color: black;
  font-weight: bold;
}

/* Baterija */
.battery {
  width: 25px;
  height: 12px;
  border: 2px solid black;
  border-radius: 3px;
  position: relative;
}

.battery::after {
  content: '';
  width: 2px;
  height: 6px;
  background: black;
  position: absolute;
  top: 2px;
  right: -4px;
  border-radius: 1px;
}

.battery-level {
  width: 18px;
  height: 100%;
  background: black;
  border-radius: 1px;
}

/* App ekrāns */
.app-screen {
  flex: 1;
  background: #f5f5f7;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

/* Kartītes centrētas */
.card {
  background: #fff;
  padding: 30px;
  border-radius: 30px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  width: 320px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Virsraksti */
h1 {
  text-align: center;
  color: #007aff;
  margin-bottom: 25px;
  font-size: 28px;
}

/* Input un textarea */
input, textarea {
  width: 100%;
  padding: 14px;
  border-radius: 12px;
  border: 1px solid #ccc;
  margin-bottom: 15px;
  font-size: 16px;
}

input:focus, textarea:focus {
  border-color: #007aff;
  box-shadow: 0 0 5px rgba(0,122,255,0.5);
}

textarea {
  resize: none;
  min-height: 80px;
}

/* Pogas */
button {
  width: 100%;
  padding: 14px;
  border-radius: 14px;
  border: none;
  background: #007aff;
  color: white;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  margin-bottom: 10px;
  transition: 0.3s;
}

button:hover {
  background: #005bb5;
}

.logout-btn {
  background: #ff3b30;
}

.logout-btn:hover {
  background: #c1271c;
}

/* Ziņu kartītes */
.news {
  background: #fff;
  padding: 15px;
  border-radius: 20px;
  margin-bottom: 15px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.08);
}

.news h3 {
  color: #007aff;
  margin-bottom: 5px;
}

.news p {
  color: #555;
  font-size: 14px;
}
</style>
</head>
<body>

<div class="iphone-frame">

  <div class="camera-notch"></div>

  <div class="status-bar">
    <span class="time">9:41</span>
    <div class="battery">
      <div class="battery-level"></div>
    </div>
  </div>

  <div class="app-screen">

    <div id="auth" class="card">

      <h1>Skolas Ziņu Centrs</h1>

      <h2>Reģistrācija</h2>
      <input id="regEmail" placeholder="E-pasts">
      <input id="regPass" type="password" placeholder="Parole">
      <button onclick="register()">Reģistrēties</button>

      <h2>Pieslēgšanās</h2>
      <input id="loginEmail" placeholder="E-pasts">
      <input id="loginPass" type="password" placeholder="Parole">
      <button onclick="login()">Pieslēgties</button>

    </div>

    <div id="app" class="card" style="display:none;">

      <button class="logout-btn" onclick="logout()">Iziet</button>

      <h2>Publicēt paziņojumu</h2>
      <input id="title" placeholder="Virsraksts">
      <input id="className" placeholder="Klase">
      <textarea id="text" placeholder="Ziņas teksts"></textarea>
      <button onclick="addNews()">Publicēt</button>

      <h2>Paziņojumi</h2>
      <div id="newsList"></div>

    </div>

  </div>

</div>

<script>
let users = [];
let news = [];

function register() {
  let email = document.getElementById("regEmail").value;
  let pass = document.getElementById("regPass").value;
  if(!email || !pass){ alert("Aizpildi visus laukus!"); return; }
  if(users.find(u=>u.email===email)){ alert("Šis e-pasts jau reģistrēts!"); return; }
  users.push({email, pass});
  alert("Reģistrācija veiksmīga!");
  document.getElementById("regEmail").value="";
  document.getElementById("regPass").value="";
}

function login() {
  let email = document.getElementById("loginEmail").value;
  let pass = document.getElementById("loginPass").value;
  let user = users.find(u=>u.email===email && u.pass===pass);
  if(user){
    document.getElementById("auth").style.display="none";
    document.getElementById("app").style.display="flex";
  } else {
    alert("Nepareizi dati!");
  }
}

function logout(){
  document.getElementById("auth").style.display="flex";
  document.getElementById("app").style.display="none";
}

function addNews(){
  let title = document.getElementById("title").value;
  let text = document.getElementById("text").value;
  let className = document.getElementById("className").value;
  if(!title || !text || !className){ alert("Aizpildi visus laukus!"); return; }
  news.push({title,text,className});
  renderNews(news);
  document.getElementById("title").value="";
  document.getElementById("text").value="";
  document.getElementById("className").value="";
}

function renderNews(arr){
  let list = document.getElementById("newsList");
  list.innerHTML = "";
  arr.forEach(n=>{
    list.innerHTML += `<div class="news">
      <h3>${n.title}</h3>
      <p>${n.text}</p>
      <p>Klase: ${n.className}</p>
    </div>`;
  });
}
</script>

</body>
</html>
