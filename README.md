<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Nowshad's Macro Calculator</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<style>
/* ---------- THEME ---------- */
:root{
  --bg1:#0b3aa8; --bg2:#071a4b;
  --card:rgba(255,255,255,.10);
  --stroke:rgba(255,255,255,.18);
  --text:#eef3ff; --muted:#b9c7ff;
  --good:#3ddc97; --warn:#ffcc66; --bad:#ff6b6b;
  --radius:18px;
}
*{box-sizing:border-box}
body{
  margin:0;
  font-family:Inter,system-ui;
  color:var(--text);
  background:linear-gradient(145deg,var(--bg1),var(--bg2));
}
.wrap{max-width:1100px;margin:auto;padding:14px}

/* ---------- NAV ---------- */
.topbar{
  display:flex;justify-content:space-between;align-items:center;
  background:var(--card);border:1px solid var(--stroke);
  border-radius:var(--radius);padding:14px;
}
.tabs button{
  margin-left:6px;padding:8px 12px;border-radius:12px;
  background:rgba(255,255,255,.12);border:1px solid var(--stroke);
  color:white;font-weight:700;cursor:pointer;
}
.tabs button.active{background:rgba(255,255,255,.25)}

/* ---------- CARDS ---------- */
.card{
  background:var(--card);border:1px solid var(--stroke);
  border-radius:var(--radius);padding:14px;margin-top:14px;
}
h2{margin:0 0 10px;font-size:16px}
label{font-size:12px;color:var(--muted)}
input,select{
  width:100%;padding:10px;border-radius:12px;
  background:rgba(10,20,60,.6);border:1px solid var(--stroke);
  color:white;font-size:14px;
}
.row{display:grid;gap:10px}
@media(min-width:720px){.row.cols2{grid-template-columns:1fr 1fr}}
@media(min-width:920px){.row.cols3{grid-template-columns:1fr 1fr 1fr}}
.hide{display:none}

/* ---------- KPI ---------- */
.kpi{
  background:rgba(255,255,255,.12);
  border-radius:14px;padding:12px;
}
.kpi .big{font-size:18px;font-weight:800}
.good{color:var(--good)} .bad{color:var(--bad)}
canvas{max-width:100%}

/* ---------- TABLE ---------- */
table{width:100%;border-collapse:collapse;font-size:12px}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,.1)}
button{cursor:pointer}
</style>
</head>

<body>
<div class="wrap">

<!-- ================= NAV ================= -->
<div class="topbar">
  <div>
    <b>Nowshad's Macro Calculator</b><br>
    <small>Profile → Food → Lifestyle → Dashboard</small>
  </div>
  <div class="tabs">
    <button class="active" onclick="showTab('dash')">Dashboard</button>
    <button onclick="showTab('profile')">Profile</button>
    <button onclick="showTab('food')">Food</button>
    <button onclick="showTab('life')">Lifestyle</button>
  </div>
</div>

<!-- ================= DASHBOARD ================= -->
<div id="dash" class="card">
<h2>Dashboard</h2>
<div class="row cols3">
  <div class="kpi"><div>Calories</div><div class="big" id="dCal">0</div><small>Target: <span id="dCalT">0</span></small></div>
  <div class="kpi"><div>Protein</div><div class="big" id="dProt">0 g</div><small>Target: <span id="dProtT">0</span></small></div>
  <div class="kpi"><div>Net Balance</div><div class="big" id="dBal">—</div></div>
</div>

<div class="row cols3" style="margin-top:10px">
  <div class="kpi"><div>Carbs</div><div class="big" id="dCarb">0 g</div><small>Target: <span id="dCarbT">0</span></small></div>
  <div class="kpi"><div>Fats</div><div class="big" id="dFat">0 g</div><small>Target: <span id="dFatT">0</span></small></div>
  <div class="kpi"><div>BMI • BMR • TDEE</div><div class="big" id="dMeta">—</div></div>
</div>

<div style="margin-top:14px">
<canvas id="chart" height="220"></canvas>
</div>
</div>

<!-- ================= PROFILE ================= -->
<div id="profile" class="card hide">
<h2>Profile</h2>

<div class="row cols2">
  <div><label>Name</label><input id="name"></div>
  <div><label>Age</label><input id="age" type="number"></div>
</div>

<div class="row cols2">
  <div><label>Height (cm)</label><input id="height" type="number"></div>
  <div><label>Weight (kg)</label><input id="weight" type="number"></div>
</div>

<div class="row cols2">
  <div>
    <label>Goal</label>
    <select id="goal">
      <option value="loss">Weight Loss</option>
      <option value="maintain">Maintenance</option>
      <option value="gain">Weight Gain</option>
    </select>
  </div>
  <div>
    <label>Activity Level</label>
    <select id="activity">
      <option value="1.2">Sedentary</option>
      <option value="1.375">Light (1–3 days)</option>
      <option value="1.55">Moderate (3–5 days)</option>
      <option value="1.725">Active (6–7 days)</option>
    </select>
  </div>
</div>

<div class="row cols3">
  <div><label>Protein multiplier (g/kg)</label><input id="pm" value="2.2"></div>
  <div><label>Target Calories</label><input id="tCal"></div>
  <div><label>Target Fats (g)</label><input id="tFat"></div>
</div>

<div class="row cols2">
  <div><label>Target Protein (auto)</label><input id="tProt" readonly></div>
  <div><label>Target Carbs (auto)</label><input id="tCarb" readonly></div>
</div>

<button onclick="saveProfile()">Save Profile</button>
</div>

<!-- ================= FOOD ================= -->
<div id="food" class="card hide">
<h2>Food Log</h2>

<div class="row cols3">
  <select id="foodItem"></select>
  <input id="qty" type="number" placeholder="Quantity">
  <button onclick="addFood()">Add</button>
</div>

<table>
<thead><tr><th>Food</th><th>Qty</th><th>kcal</th><th>P</th><th>C</th><th>F</th><th></th></tr></thead>
<tbody id="foodTable"></tbody>
</table>
</div>

<!-- ================= LIFESTYLE ================= -->
<div id="life" class="card hide">
<h2>Lifestyle</h2>

<div class="row cols3">
  <input id="sleep" type="number" placeholder="Sleep (hrs)">
  <input id="water" type="number" placeholder="Water (L)">
  <input id="burn" type="number" placeholder="Workout kcal burned">
</div>

<button onclick="saveLife()">Save Lifestyle</button>
</div>

</div>

<script>
/* ================= DATA ================= */
const FOOD_DB={
 "Chicken Breast":{k:165,p:31,c:0,f:3.6},
 "Rice":{k:130,p:2.7,c:28,f:0.3},
 "Egg":{k:72,p:6,c:0.6,f:5},
 "Olive Oil":{k:119,p:0,c:0,f:13.5},
 "Banana":{k:105,p:1.3,c:27,f:0.4}
};

/* ================= STATE ================= */
let foods=[];
let chart;

/* ================= NAV ================= */
function showTab(id){
 document.querySelectorAll('.card').forEach(c=>c.classList.add('hide'));
 document.getElementById(id).classList.remove('hide');
 document.querySelectorAll('.tabs button').forEach(b=>b.classList.remove('active'));
}

/* ================= PROFILE LOGIC ================= */
function calcProfile(){
 const w=+weight.value,h=+height.value,a=+age.value;
 if(!w||!h||!a) return null;
 const bmi=(w/((h/100)**2));
 const bmr=10*w+6.25*h-5*a+5;
 const tdee=bmr*(+activity.value);
 const adj=goal.value==="gain"?400:goal.value==="loss"?-500:0;
 const tCalAuto=Math.round(tdee+adj);

 const prot=Math.round(w*(+pm.value));
 const fat=Math.round((tCalAuto*0.25)/9);
 const carb=Math.round((tCalAuto-prot*4-fat*9)/4);

 tProt.value=prot;
 tCarb.value=carb;
 if(!tCal.value) tCal.value=tCalAuto;
 if(!tFat.value) tFat.value=fat;

 return {bmi,bmr,tdee,prot,carb,fat,cal:+tCal.value};
}

function saveProfile(){
 localStorage.profile=JSON.stringify(calcProfile());
 refresh();
}

/* ================= FOOD ================= */
function addFood(){
 const f=foodItem.value,q=+qty.value;
 if(!f||!q) return;
 const d=FOOD_DB[f];
 foods.push({f,q,...d});
 renderFood();
 refresh();
}

function renderFood(){
 foodTable.innerHTML='';
 foods.forEach((x,i)=>{
  foodTable.innerHTML+=`<tr>
  <td>${x.f}</td><td>${x.q}</td>
  <td>${x.k*x.q}</td><td>${x.p*x.q}</td>
  <td>${x.c*x.q}</td><td>${x.f*x.q}</td>
  <td><button onclick="foods.splice(${i},1);renderFood();refresh()">x</button></td></tr>`;
 });
}

/* ================= DASHBOARD ================= */
function refresh(){
 const p=calcProfile();
 if(!p) return;
 const t=foods.reduce((a,b)=>({
  k:a.k+b.k*b.q,p:a.p+b.p*b.q,c:a.c+b.c*b.q,f:a.f+b.f*b.q
 }),{k:0,p:0,c:0,f:0});

 dCal.innerText=t.k;
 dCalT.innerText=p.cal;
 dProt.innerText=t.p+" g";
 dProtT.innerText=p.prot;
 dCarb.innerText=t.c+" g";
 dCarbT.innerText=p.carb;
 dFat.innerText=t.f+" g";
 dFatT.innerText=p.fat;
 dMeta.innerText=`BMI ${p.bmi.toFixed(1)} • BMR ${Math.round(p.bmr)} • TDEE ${Math.round(p.tdee)}`;

 const diff=t.k-p.cal;
 dBal.innerText=(diff>0?'+':'')+diff;
 dBal.className='big '+(diff>0?'bad':'good');

 chart.data.datasets[0].data=[t.p,t.c,t.f];
 chart.update();
}

/* ================= INIT ================= */
foodItem.innerHTML=Object.keys(FOOD_DB).map(x=>`<option>${x}</option>`).join('');
chart=new Chart(document.getElementById('chart'),{
 type:'bar',
 data:{labels:['Protein','Carbs','Fats'],
 datasets:[{data:[0,0,0],backgroundColor:['#3ddc97','#ffcc66','#ff6b6b']}]},
 options:{responsive:true,maintainAspectRatio:false}
});
</script>
</body>
</html>
