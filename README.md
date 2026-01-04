<!DOCTYPE html>
<html lang="hu">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width">
<title>BetFr</title>

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
textarea{width:100%;height:140px;background:#111;color:#fff;border-radius:10px;padding:10px}
button.action{width:100%;padding:14px;margin:8px 0;border:none;border-radius:12px;font-weight:bold}
.win{background:lime}
.lost{background:red;color:#fff}
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

<!-- FREE -->
<section id="free" class="active">
<h2>🎯 FREE TIPS</h2>
<div id="freeTips"></div>
</section>

<!-- VIP -->
<section id="vip">
<h2>👑 VIP TIPS</h2>
<div id="vipTips"></div>
</section>

<!-- ADMIN -->
<section id="admin">
<h2>⚙️ ADMIN PANEL</h2>

<h3>FREE TIPPEK</h3>
<textarea id="freeInput"></textarea>
<button class="action free" onclick="saveTips('free')">FREE MENTÉS</button>

<h3>VIP TIPPEK</h3>
<textarea id="vipInput"></textarea>
<button class="action vip" onclick="saveTips('vip')">VIP MENTÉS</button>

<h3>EREDMÉNY</h3>
<button class="action win" onclick="setResult('WIN')">✅ WIN</button>
<button class="action lost" onclick="setResult('LOST')">❌ LOST</button>
</section>

<script>
/* ====== ALAP ====== */
let lang = localStorage.getItem("lang") || "hu";
const adminPass="admin123";
const vipPass="vip123";

/* ====== NAV ====== */
function show(id){
 document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));
 document.getElementById(id).classList.add("active");
 loadTips();
}

/* ====== VIP LOGIN ====== */
function vipLogin(){
 let p=prompt("VIP PASSWORD");
 if(p===vipPass) show("vip");
 else alert("Hibás jelszó");
}

/* ====== ADMIN LOGIN ====== */
function adminLogin(){
 let p=prompt("ADMIN PASSWORD");
 if(p===adminPass) show("admin");
 else alert("Hibás jelszó");
}

/* ====== TIPPEK ====== */
function saveTips(type){
 localStorage.setItem(type+"Tips", document.getElementById(type+"Input").value);
 alert("Mentve!");
 loadTips();
}

function setResult(r){
 localStorage.setItem("result",r);
 alert("Eredmény: "+r);
 loadTips();
}

function loadTips(){
 let free = localStorage.getItem("freeTips") || "Nincs FREE tipp";
 let vip  = localStorage.getItem("vipTips")  || "Nincs VIP tipp";
 let res  = localStorage.getItem("result")   || "⏳ PENDING";

 function renderCards(text){
  return text
    .split(/\n\s*\n/)   // üres sorokra bont
    .map(t => 
      '<div style="background:#111;padding:14px;margin:12px 0;border-radius:14px;box-shadow:0 6px 16px rgba(0,0,0,.6)">' +
      t.replace(/\n/g,"<br>") +
      '</div>'
    ).join("");
}

freeTips.innerHTML = renderCards(free) + "<b>"+res+"</b>";
vipTips.innerHTML  = renderCards(vip)  + "<b>"+res+"</b>";

 freeInput.value = free;
 vipInput.value  = vip;
}

/* ====== NYELV ====== */
function toggleLang(){
 lang = lang==="hu"?"en":"hu";
 localStorage.setItem("lang",lang);
 alert(lang==="hu"?"Magyar":"English");
}

/* ====== START ====== */
loadTips();
</script>

</body>
</html>
