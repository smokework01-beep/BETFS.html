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
#vip-section{display:none;}
#vip-login{margin:15px;padding:15px;background:#222;border-radius:15px;}
input[type=password]{padding:10px;width:70%;border-radius:10px;border:none;margin-right:10px;}
button{padding:10px;border-radius:10px;border:none;background:#1e90ff;color:#fff;cursor:pointer;}
</style>
</head>
<body>

<header>⚽ BetFr – Free & VIP Tips</header>

<div style="text-align:center;">
  <div class="nav-btn" onclick="showFree()">FREE</div>
  <div class="nav-btn" onclick="showVIP()">VIP</div>
</div>

<div id="free-section"></div>

<div id="vip-login">
  <p>VIP jelszó:</p>
  <input type="password" id="vip-pass" placeholder="Írd be a jelszót">
  <button onclick="checkVIP()">Belépés</button>
</div>

<div id="vip-section"></div>
<div id="stats" class="stat"></div>

<script>
// 🔹 JSON fájlok GitHub Pages linkjei
const FREE_URL = "https://FELHASZNALONEV.github.io/REPO/free.json";
const VIP_URL  = "https://FELHASZNALONEV.github.io/REPO/vip.json";

// 🔹 VIP jelszó
const VIP_PASSWORD = "Bet794590";

// 🔹 Betöltés funkció
function loadTips(url, containerId) {
  fetch(url)
  .then(res => res.json())
  .then(data => {
    let win=0,lost=0,pending=0;
    const container = document.getElementById(containerId);
    container.innerHTML = "";
    data.forEach(t => {
      if(t.result==="WIN") win++;
      else if(t.result==="LOST") lost++;
      else pending++;

      container.innerHTML += `
        <div class="card">
          <b>${t.match}</b><br><br>
          🎯 Tipp: ${t.tip}<br>
          💰 Odds: ${t.odds}<br>
          📅 Dátum: ${t.date}<br><br>
          Eredmény:
          <span class="${t.result.toLowerCase()}">${t.result}</span>
        </div>
      `;
    });
    document.getElementById("stats").innerHTML = `
      📊 Összes: ${data.length}<br>
      ✅ WIN: ${win}<br>
      ❌ LOST: ${lost}<br>
      ⏳ PENDING: ${pending}<br>
      📈 Win %: ${Math.round(win/(win+lost||1)*100)}%
    `;
  })
  .catch(()=>{
    document.getElementById(containerId).innerHTML="<p style='padding:20px'>Nem sikerült betölteni a tippeket</p>";
  });
}

// 🔹 FREE rész mutatása
function showFree(){
  document.getElementById("free-section").style.display="block";
  document.getElementById("vip-section").style.display="none";
  document.getElementById("vip-login").style.display="none";
  loadTips(FREE_URL,"free-section");
}

// 🔹 VIP rész mutatása
function showVIP(){
  document.getElementById("vip-login").style.display="block";
  document.getElementById("free-section").style.display="none";
  document.getElementById("vip-section").style.display="none";
}

// 🔹 VIP jelszó ellenőrzés
function checkVIP(){
  const pass = document.getElementById("vip-pass").value;
  if(pass === VIP_PASSWORD){
    document.getElementById("vip-login").style.display="none";
    document.getElementById("vip-section").style.display="block";
    loadTips(VIP_URL,"vip-section");
  }else{
    alert("Hibás jelszó!");
  }
}

// 🔹 Alapértelmezett: FREE
showFree();
</script>

</body>
</html>
