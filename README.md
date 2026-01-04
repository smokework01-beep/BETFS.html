<!DOCTYPE html>
<html lang="hu">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width">
<title>BetFr – Free & VIP Tips</title>
<style>
body{margin:0;font-family:sans-serif;background:#000;color:#fff;}
header{padding:15px;text-align:center;font-size:26px;font-weight:bold;background:#111;}
.nav-btn{width:48%;display:inline-block;margin:1%;padding:15px;text-align:center;border-radius:15px;background:#222;cursor:pointer;}
.card{background:#111;margin:15px;padding:15px;border-radius:15px;box-shadow:0 6px 20px rgba(0,0,0,.6);}
.win{color:#00ff6a;font-weight:bold;}
.lost{color:#ff4d4d;font-weight:bold;}
.pending{color:#ffd700;font-weight:bold;}
.stat{background:#1a1a1a;margin:15px;padding:15px;border-radius:15px;}
#vip-section,#admin-section{display:none;}
#vip-login,#admin-login{margin:15px;padding:15px;background:#222;border-radius:15px;}
input[type=password],input[type=text]{padding:10px;width:65%;border-radius:10px;border:none;margin-right:10px;margin-bottom:10px;}
button{padding:10px;border-radius:10px;border:none;background:#1e90ff;color:#fff;cursor:pointer;}
select{padding:8px;border-radius:10px;margin-right:10px;border:none;}
</style>
</head>
<body>

<header>⚽ BetFr – Free & VIP Tips</header>

<div style="text-align:center;">
  <div class="nav-btn" onclick="showFree()">FREE</div>
  <div class="nav-btn" onclick="showVIP()">VIP</div>
  <div class="nav-btn" onclick="showAdmin()">ADMIN</div>
</div>

<!-- Free section -->
<div id="free-section"></div>

<!-- VIP login -->
<div id="vip-login">
  <p>VIP jelszó:</p>
  <input type="password" id="vip-pass" placeholder="Írd be a jelszót">
  <button onclick="checkVIP()">Belépés</button>
</div>

<!-- VIP section -->
<div id="vip-section"></div>

<!-- Admin login -->
<div id="admin-login">
  <p>Admin jelszó:</p>
  <input type="password" id="admin-pass" placeholder="Írd be az admin jelszót">
  <button onclick="checkAdmin()">Belépés</button>
</div>

<!-- Admin section -->
<div id="admin-section">
  <h3>Admin Panel</h3>
  <input type="text" id="new-match" placeholder="Mérkőzés">
  <input type="text" id="new-tip" placeholder="Tipp">
  <input type="text" id="new-odds" placeholder="Odds">
  <select id="new-result">
    <option value="PENDING">PENDING</option>
    <option value="WIN">WIN</option>
    <option value="LOST">LOST</option>
  </select>
  <select id="new-type">
    <option value="free">FREE</option>
    <option value="vip">VIP</option>
  </select>
  <button onclick="addTip()">Hozzáadás</button>
</div>

<div id="stats" class="stat"></div>

<script>
// JSON fájlok GitHub Pages linkjei
let FREE_URL = "https://FELHASZNALONEV.github.io/REPO/free.json";
let VIP_URL  = "https://FELHASZNALONEV.github.io/REPO/vip.json";

// Jelszavak
const VIP_PASSWORD = "Bet794590";
const ADMIN_PASSWORD = "Admin79459";

// Tippek tárolása a memóriában (localStorage is lehet)
let tipsData = [];

// 🔹 Betöltés funkció
function loadTips(url, containerId){
  fetch(url)
  .then(res => res.json())
  .then(data=>{
    tipsData = data; // memória
    renderTips(data, containerId);
  })
  .catch(()=>{
    document.getElementById(containerId).innerHTML="<p style='padding:20px'>Nem sikerült betölteni a tippeket</p>";
  });
}

// 🔹 Megjelenítés kártyákkal + statisztika
function renderTips(data, containerId){
  let win=0,lost=0,pending=0;
  const container=document.getElementById(containerId);
  container.innerHTML="";
  data.forEach(t=>{
    if(t.result==="WIN") win++;
    else if(t.result==="LOST") lost++;
    else pending++;
    container.innerHTML += `
      <div class="card">
        <b>${t.match}</b><br><br>
        🎯 Tipp: ${t.tip}<br>
        💰 Odds: ${t.odds}<br>
        📅 Dátum: ${t.date || 'N/A'}<br><br>
        Eredmény:
        <span class="${t.result.toLowerCase()}">${t.result}</span><br>
        Típus: ${t.type.toUpperCase()}
      </div>
    `;
  });
  document.getElementById("stats").innerHTML=`
    📊 Összes: ${data.length}<br>
    ✅ WIN: ${win}<br>
    ❌ LOST: ${lost}<br>
    ⏳ PENDING: ${pending}<br>
    📈 Win %: ${Math.round(win/(win+lost||1)*100)}%
  `;
}

// 🔹 Free section mutatása
function showFree(){
  document.getElementById("free-section").style.display="block";
  document.getElementById("vip-section").style.display="none";
  document.getElementById("vip-login").style.display="none";
  document.getElementById("admin-login").style.display="none";
  document.getElementById("admin-section").style.display="none";
  loadTips(FREE_URL,"free-section");
}

// 🔹 VIP section mutatása
function showVIP(){
  document.getElementById("vip-login").style.display="block";
  document.getElementById("free-section").style.display="none";
  document.getElementById("vip-section").style.display="none";
  document.getElementById("admin-login").style.display="none";
  document.getElementById("admin-section").style.display="none";
}

// 🔹 Admin panel mutatása
function showAdmin(){
  document.getElementById("admin-login").style.display="block";
  document.getElementById("free-section").style.display="none";
  document.getElementById("vip-section").style.display="none";
}

// 🔹 VIP jelszó ellenőrzés
function checkVIP(){
  const pass = document.getElementById("vip-pass").value;
  if(pass===VIP_PASSWORD){
    document.getElementById("vip-login").style.display="none";
    document.getElementById("vip-section").style.display="block";
    loadTips(VIP_URL,"vip-section");
  } else { alert("Hibás VIP jelszó!"); }
}

// 🔹 Admin jelszó ellenőrzés
function checkAdmin(){
  const pass = document.getElementById("admin-pass").value;
  if(pass===ADMIN_PASSWORD){
    document.getElementById("admin-login").style.display="none";
    document.getElementById("admin-section").style.display="block";
    renderTips(tipsData,"admin-section"); // admin panel látja a tippeket
  } else { alert("Hibás Admin jelszó!"); }
}

// 🔹 Admin hozzáad tippeket
function addTip(){
  const match=document.getElementById("new-match").value;
  const tip=document.getElementById("new-tip").value;
  const odds=document.getElementById("new-odds").value;
  const result=document.getElementById("new-result").value;
  const type=document.getElementById("new-type").value;
  if(!match||!tip||!odds) { alert("Tölts ki minden mezőt!"); return; }
  const newTip={match,tip,odds,result,type,date:new Date().toISOString().slice(0,10)};
  tipsData.push(newTip);
  renderTips(tipsData,"admin-section");
  alert("Tipp hozzáadva! ⚡\nMegjegyzés: GitHub JSON frissítés külön szükséges.");
}

// 🔹 Alapértelmezett: FREE
showFree();
</script>

</body>
</html>
