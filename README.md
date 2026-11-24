# guai-guai-card-2026
乖乖集點卡｜20 點兌換禮物
index.html


<!DOCTYPE html>

<html lang="zh-TW">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1">

<title>乖乖集點卡 ♡</title>

<style>

*{box-sizing:border-box}

body{

 margin:0;padding:24px 15px;

 background:#f4e9d9;color:#654b38;

 font-family:"Noto Sans TC",sans-serif;

 text-align:center;

}

.card{

 max-width:430px;margin:18px auto;

 padding:25px 20px;background:#fffaf2;

 border:2px solid #ead8c2;

 border-radius:28px;

 box-shadow:0 8px 25px #87684b20;

}

.bear{font-size:75px;margin:8px}

h1{font-size:30px;margin:8px}

h3{margin:8px 0 15px}

.sub{color:#a1846c;font-size:14px}

.points{font-size:55px;font-weight:900;color:#ad7952;margin:18px}

.bar{height:15px;background:#eee0cd;border-radius:20px;overflow:hidden}

.fill{height:100%;width:0;background:#bd936c;transition:.3s}

button{

 border:0;border-radius:15px;padding:13px 18px;

 background:#a77a55;color:white;font-size:15px;

 margin:7px;cursor:pointer;

}

.gray{background:#e9d9c5;color:#654b38}

input{

 padding:13px;border:1px solid #ddc9b1;

 border-radius:12px;width:90%;margin:7px;

}

.record{padding:11px;border-bottom:1px dashed #ddc9b1;font-size:13px}

.hidden{display:none}

.gift{background:#f8eee5;padding:15px;border-radius:18px;margin-top:20px}

.notice{font-size:12px;color:#a1846c;line-height:1.6}

</style>

</head>

<body>

<div class="card">

 <div class="bear">🐻</div>

 <h1>乖乖集點卡 ♡</h1>

 <p class="sub">每天乖乖起床，收集小小獎勵</p>

 <div class="points">

  <span id="points">0</span>

  <span style="font-size:20px">/ 20 點</span>

 </div>

 <div class="bar">

  <div class="fill" id="fill"></div>

 </div>

 <p id="remaining">還差 20 點就能兌換禮物！</p>

 <div class="gift">

  🎁 <b>集滿 20 點</b><br>

  就可以兌換一份神祕小禮物 ♡

 </div>

</div>

<div class="card">

 <h3>📖 集點紀錄</h3>

 <div id="records">目前還沒有紀錄</div>

</div>

<div class="card">

 <h3>🔐 管理室</h3>

 <div id="login">

  <input id="password" type="password"

   placeholder="請輸入管理密碼">

  <button onclick="login()">登入管理室</button>

 </div>

 <div id="admin" class="hidden">

  <p class="sub">只有管理者可以操作點數</p>

  <button onclick="addPoint()">＋ 增加 1 點</button>

  <button class="gray" onclick="removePoint()">－ 扣除 1 點</button>

  <button class="gray" onclick="redeem()">🎁 兌換禮物</button>

  <button class="gray" onclick="logout()">登出</button>

 </div>

 <p class="notice">

  此版本資料只存在目前這台裝置的瀏覽器。

  尚未啟用雲端同步及真正的管理者權限。

 </p>

</div>

<script>

const ADMIN_PASSWORD="123456";

let points=Number(localStorage.getItem("points")||0);

let records=JSON.parse(localStorage.getItem("records")||"[]");

function save(){

 localStorage.setItem("points",points);

 localStorage.setItem("records",JSON.stringify(records));

 render();

}

function addRecord(text,delta){

 records.push({

  text:text,

  delta:delta,

  date:new Date().toLocaleString("zh-TW")

 });

}

function render(){

 document.getElementById("points").textContent=points;

 document.getElementById("fill").style.width=

 Math.min(points/20*100,100)+"%";

 document.getElementById("remaining").textContent=

 points>=20

 ?"🎉 集滿了！可以兌換禮物！"

 :"還差 "+(20-points)+" 點就能兌換禮物！";

 document.getElementById("records").innerHTML=

 records.length

 ?records.slice().reverse().map(r=>

  `<div class="record">

   ${r.date}<br>${r.text}

  </div>`

 ).join("")

 :"目前還沒有紀錄";

}

function login(){

 const p=document.getElementById("password").value;

 if(p===ADMIN_PASSWORD){

  document.getElementById("login").classList.add("hidden");

  document.getElementById("admin").classList.remove("hidden");

  alert("管理者登入成功！");

 }else{

  alert("密碼錯誤！");

 }

}

function logout(){

 document.getElementById("login").classList.remove("hidden");

 document.getElementById("admin").classList.add("hidden");

 document.getElementById("password").value="";

}

function addPoint(){

 points++;

 addRecord("乖乖起床，獲得 1 點",1);

 save();

}

function removePoint(){

 if(points<=0)return alert("目前沒有點數可以扣除！");

 if(!confirm("確定扣除 1 點嗎？"))return;

 points--;

 addRecord("管理者調整，扣除 1 點",-1);

 save();

}

function redeem(){

 if(points<20)return alert("還沒有集滿 20 點喔！");

 if(!confirm("確定兌換一份禮物嗎？"))return;

 points-=20;

 addRecord("🎁 兌換一份神祕禮物",-20);

 save();

}

render();

</script>

</body>

</html>