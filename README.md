<!DOCTYPE html>
<html lang="hu">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width">
<title>BetFr</title>

<!-- PWA Manifest -->
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#000000">

<style>
body{margin:0;background:#000;color:#fff;font-family:sans-serif}
header{padding:15px;text-align:center;font-size:26px;font-weight:bold;background:#111}
nav{display:flex}
nav button{flex:1;padding:14px;border:none;font-weight:bold;font-size:16px}
.free{background:#FFD700}
.vip{background:#9370DB}
.admin{background:#4169E1;color:#fff}
.lang{background:#333;color:#fff}
section{display:none;padding:20px}
.active{display:block}
textarea{width:100%;height:140px;background:#111;color:#fff;border-radius:10px;padding:10px;margin-bottom:10px}
input{width:100%;padding:10px;margin:6px 0;border-radius:8px;border:none;background:#111;color:#fff}
button.action{width:100%;padding:14px;margin:8px 0;border:none;border-radius:12px;font-weight:bold}
.win{background:lime}
.lost{background:red;color:#fff}
.card{background:#111;padding:14px;margin:12px 0;border-radius:14px;box-shadow:0 6px 16px rgba(0,0,0,.6)}
.stat{margin-top:10px;padding:12px;background:#222;border-radius:12px}
.card button{float:right;margin-left:6px;padding:3px 8px;border-radius:8px;font-weight:bold;border:none;cursor:pointer}
</style>
</head>

<body>

<header>⚽ BetFr</header>

<nav>
<button class="free" onclick="show('free')">FREE</button>
<button class="vip" onclick="vipLogin()">VIP</button>
<button class="admin" onclick="adminLogin()">ADMIN</button>
<button class="lang" onclick="toggleLang()">HU / EN</button>
</nav>

<section id="free" class="active">
<h2>🎯 FREE TIPS</h2>
<div id="freeTips"></div>
<div class="stat" id="freeStat"></div>
</section>

<section id="vip">
<h2>👑 VIP TIPS</h2>
<div id="vipTips"></div>
<div class="stat" id="vipStat"></div>
</section>

<section id="admin">
<h2>⚙️ ADMIN PANEL</h2>

<h3>FREE TIPPEK</h3>
<textarea id="freeInput" placeholder="Ide írd a FREE tippeket, üres sor = új tipp"></textarea>
<button class="action free" onclick="saveTips('free')">FREE MENTÉS</button>

<h3>VIP TIPPEK</h3>
<textarea id="vipInput" placeholder="Ide írd a VIP tippeket, üres sor = új tipp"></textarea>
<button class="action vip" onclick="saveTips('vip')">VIP MENTÉS</button>

<h3>🔒 JELSZÓ BEÁLLÍTÁS</h3>
<label>ADMIN jelszó:</label>
<input type="text" id="adminNewPass" placeholder="Új admin jelszó">
<label>VIP jelszó:</label>
<input type="text" id="vipNewPass" placeholder="Új VIP jelszó">
<button class="action admin" onclick="savePasswords()">MENTÉS</button>
</section>

<script>
// Alapértelmezett jelszavak
let adminPass = "Admin79459";
let vipPass   = "Bet794590";

// Ellenőrizzük localStorage-ban tárolt jelszót
if(localStorage.getItem("adminPass")) adminPass = localStorage.getItem("adminPass");
if(localStorage.getItem("vipPass"))   vipPass   = localStorage.getItem("vipPass");

let lang = localStorage.getItem("lang") || "hu";

/* ====== NAV + LOGIN ====== */
function show(id){
 document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));
 document.getElementById(id).classList.add("active");
 loadTips();
}

function vipLogin(){
 let p=prompt("VIP PASSWORD");
 if(p===vipPass) show("vip");
 else alert("Hibás jelszó");
}

function adminLogin(){
 let p=prompt("ADMIN PASSWORD");
 if(p===adminPass) show("admin");
 else alert("Hibás jelszó");
}

/* ====== TIPPEK MENTÉSE ====== */
function saveTips(type){
 localStorage.setItem(type+"Tips", document.getElementById(type+"Input").value);
 let statuses = {};
 localStorage.setItem(type+"Status", JSON.stringify(statuses));
 alert("Mentve!");
 loadTips();
}

/* ====== KÁRTYÁS TIPPEK + WIN/LOST ====== */
function renderCards(text,type){
 let statuses = JSON.parse(localStorage.getItem(type+"Status")||"{}");
 let tArr = text.split(/\n\s*\n/);
 return tArr.map((t,i)=>{
  let r = statuses[i]||"⏳";
  return `<div class="card">${t.replace(/\n/g,"<br>")}
  <button class="win" onclick="setResult('${type}',${i},'WIN')">✅</button>
  <button class="lost" onclick="setResult('${type}',${i},'LOST')">❌</button>
  <div>Státusz: ${r}</div></div>`;
 }).join("");
}

function setResult(type,index,result){
 let statuses = JSON.parse(localStorage.getItem(type+"Status")||"{}");
 statuses[index]=result;
 localStorage.setItem(type+"Status", JSON.stringify(statuses));
 loadTips();
}

/* ====== STATISZTIKA ====== */
function renderStat(type){
 let statuses = JSON.parse(localStorage.getItem(type+"Status")||"{}");
 let total = Object.keys(statuses).length;
 let win = Object.values(statuses).filter(s=>"WIN"===s).length;
 let lost = Object.values(statuses).filter(s=>"LOST"===s).length;
 document.getElementById(type+"Stat").innerHTML = `Összes: ${total} | ✅ WIN: ${win} | ❌ LOST: ${lost}`;
}

/* ====== BETÖLTÉS ====== */
function loadTips(){
 let free = localStorage.getItem("freeTips")||"Nincs FREE tipp";
 let vip  = localStorage.getItem("vipTips")||"Nincs VIP tipp";
 freeTips.innerHTML = renderCards(free,"free");
 vipTips.innerHTML  = renderCards(vip,"vip");
 freeInput.value = free;
 vipInput.value  = vip;
 renderStat("free");
 renderStat("vip");
}

/* ====== NYELV ====== */
function toggleLang(){
 lang = lang==="hu"?"en":"hu";
 localStorage.setItem("lang",lang);
 alert(lang==="hu"?"Magyar":"English");
}

/* ====== JELSZÓ MENTÉSE ====== */
function savePasswords(){
  const a = document.getElementById("adminNewPass").value;
  const v = document.getElementById("vipNewPass").value;

  if(a) { localStorage.setItem("adminPass", a); adminPass = a; }
  if(v) { localStorage.setItem("vipPass", v);   vipPass = v; }

  alert("Jelszavak mentve!");
  document.getElementById("adminNewPass").value = "";
  document.getElementById("vipNewPass").value = "";
}

/* ====== START ====== */
loadTips();

/* ====== PWA SERVICE WORKER ====== */
if('serviceWorker' in navigator){
 navigator.serviceWorker.register('sw.js')
 .then(()=>console.log('SW registered'))
 .catch(err=>console.log('SW failed', err));
}
</script>
</body>
</html>
