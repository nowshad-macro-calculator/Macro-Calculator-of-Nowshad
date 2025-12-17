
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Nowshad's Macro Calculator</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <style>
    :root{
      --bg1:#0b3aa8; --bg2:#071a4b;
      --card:rgba(255,255,255,.10);
      --stroke:rgba(255,255,255,.18);
      --text:#eef3ff; --muted:rgba(238,243,255,.75);
      --good:#3ddc97; --warn:#ffcc66; --bad:#ff6b6b;
      --shadow:0 18px 55px rgba(0,0,0,.35);
      --radius:18px;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;
      color:var(--text);
      background: radial-gradient(1200px 700px at 15% 0%, #1b63ff33 0%, transparent 60%),
                  radial-gradient(900px 600px at 90% 10%, #3ddc9730 0%, transparent 55%),
                  linear-gradient(145deg, var(--bg1), var(--bg2));
      min-height:100vh;
    }
    .wrap{max-width:1100px; margin:0 auto; padding:18px 14px 70px;}
    .topbar{
      display:flex; gap:12px; align-items:center; justify-content:space-between;
      padding:14px 16px; border:1px solid var(--stroke); background:var(--card);
      border-radius:var(--radius); box-shadow:var(--shadow);
      position:sticky; top:10px; z-index:50; backdrop-filter: blur(10px);
    }
    .brand{display:flex; flex-direction:column; gap:2px}
    .brand h1{font-size:16px; margin:0; letter-spacing:.2px}
    .brand span{font-size:12px; color:var(--muted)}
    .tabs{display:flex; gap:8px; flex-wrap:wrap; justify-content:flex-end}
    .tabbtn{
      padding:10px 12px; border-radius:12px;
      border:1px solid var(--stroke); background:rgba(255,255,255,.08);
      color:var(--text); cursor:pointer; font-weight:600; font-size:13px;
      transition:.15s transform, .15s background;
    }
    .tabbtn:hover{transform:translateY(-1px); background:rgba(255,255,255,.12)}
    .tabbtn.active{background:rgba(255,255,255,.18)}
    .grid{display:grid; gap:14px}
    @media (min-width: 920px){
      .grid.cols2{grid-template-columns:1.05fr .95fr}
      .grid.cols3{grid-template-columns:1fr 1fr 1fr}
    }
    .card{
      border:1px solid var(--stroke);
      background:var(--card);
      border-radius:var(--radius);
      box-shadow:var(--shadow);
      padding:14px;
    }
    .card h2{margin:0 0 10px; font-size:15px}
    .card h3{margin:0 0 10px; font-size:14px; color:#d9e7ff}
    .sub{color:var(--muted); font-size:12px; line-height:1.35}
    .row{display:grid; gap:10px}
    @media (min-width: 640px){
      .row.cols2{grid-template-columns:1fr 1fr}
      .row.cols3{grid-template-columns:1fr 1fr 1fr}
      .row.cols4{grid-template-columns:1fr 1fr 1fr 1fr}
    }
    label{font-size:12px; color:var(--muted); display:block; margin-bottom:6px}
    input, select, textarea{
      width:100%;
      padding:11px 12px;
      border-radius:12px;
      border:1px solid rgba(255,255,255,.20);
      background:rgba(10,20,60,.45);
      color:var(--text);
      outline:none;
      font-size:14px;
    }
    input::placeholder{color:rgba(238,243,255,.55)}
    .btn{
      padding:11px 12px; border-radius:12px; border:1px solid var(--stroke);
      background:rgba(255,255,255,.14); color:var(--text);
      cursor:pointer; font-weight:700;
    }
    .btn:hover{background:rgba(255,255,255,.18)}
    .btn.danger{background:rgba(255,107,107,.18)}
    .btn.danger:hover{background:rgba(255,107,107,.26)}
    .pill{
      display:inline-flex; align-items:center; gap:8px;
      border:1px solid var(--stroke); border-radius:999px;
      padding:8px 10px; background:rgba(255,255,255,.08);
      font-size:12px; color:var(--muted)
    }
    .kpi{
      display:flex; flex-direction:column; gap:6px; padding:12px;
      border:1px solid var(--stroke); border-radius:16px;
      background:rgba(255,255,255,.10);
    }
    .kpi .big{font-size:18px; font-weight:800; letter-spacing:.2px}
    .kpi .cap{font-size:12px; color:var(--muted)}
    .accentGood{color:var(--good)}
    .accentBad{color:var(--bad)}
    .accentWarn{color:var(--warn)}
    .divider{height:1px; background:rgba(255,255,255,.14); margin:10px 0}
    .hide{display:none !important}

    .bmiBar{
      height:10px; border-radius:999px; overflow:hidden; background:rgba(255,255,255,.14);
      border:1px solid rgba(255,255,255,.16);
    }
    .bmiFill{height:100%; width:0%; background:linear-gradient(90deg, var(--bad), var(--warn), var(--good), var(--warn), var(--bad))}
    .bmiMarker{position:relative; height:14px; margin-top:6px;}
    .bmiMarker span{
      position:absolute; top:0; transform:translateX(-50%);
      font-size:11px; color:var(--muted)
    }

    .chartBox{
      height:260px;
      border:1px solid rgba(255,255,255,.16);
      border-radius:16px;
      background:rgba(255,255,255,.06);
      padding:10px;
    }
    canvas{max-width:100% !important}

    .note{
      margin-top:10px;
      padding:10px 12px;
      border-radius:14px;
      background:rgba(61,220,151,.12);
      border:1px solid rgba(61,220,151,.28);
      color:#dffaf0;
      font-size:12px;
    }

    /* Entries table */
    .tableWrap{
      border:1px solid rgba(255,255,255,.16);
      border-radius:16px;
      overflow:auto;
      background:rgba(255,255,255,.06);
    }
    table{width:100%; border-collapse:collapse; min-width:760px;}
    th, td{
      padding:10px 10px;
      border-bottom:1px solid rgba(255,255,255,.10);
      text-align:left;
      font-size:12px;
      color:rgba(238,243,255,.92);
      vertical-align:middle;
    }
    th{color:rgba(238,243,255,.75); font-weight:700; background:rgba(255,255,255,.06)}
    .miniInput{
      width:90px; padding:8px 8px; border-radius:10px; font-size:12px;
    }
    .actionBtn{
      padding:8px 10px; border-radius:10px; border:1px solid rgba(255,255,255,.18);
      background:rgba(255,255,255,.10); color:var(--text); cursor:pointer; font-weight:700; font-size:12px;
    }
    .actionBtn:hover{background:rgba(255,255,255,.14)}
  </style>
</head>

<body>
  <div class="wrap">
    <div class="topbar">
      <div class="brand">
        <h1>Nowshad's Macro Calculator</h1>
        <span>Profile → Food Log (multiple entries/day) → Dashboard + PDF</span>
      </div>
      <div class="tabs">
        <button class="tabbtn active" data-page="dashboard">Dashboard</button>
        <button class="tabbtn" data-page="profile">Profile</button>
        <button class="tabbtn" data-page="food">Food Log</button>
      </div>
    </div>

    <!-- DASHBOARD -->
    <section id="page-dashboard" class="grid cols2" style="margin-top:14px;">
      <div class="card">
        <h2>Today’s Dashboard</h2>
        <div class="sub">Log foods multiple times/day. Totals roll up automatically.</div>

        <div class="divider"></div>

        <div class="row cols3">
          <div class="kpi">
            <div class="cap">Calories eaten</div>
            <div class="big" id="dashCalories">0 kcal</div>
            <div class="cap">Target: <span id="dashCalTarget">0</span> kcal</div>
          </div>
          <div class="kpi">
            <div class="cap">Protein eaten</div>
            <div class="big" id="dashProtein">0 g</div>
            <div class="cap">Target: <span id="dashProtTarget">0</span> g</div>
          </div>
          <div class="kpi">
            <div class="cap">Carbs / Fat eaten</div>
            <div class="big"><span id="dashCarbs">0</span> g • <span id="dashFat">0</span> g</div>
            <div class="cap">Targets: <span id="dashCarbTarget">0</span> g • <span id="dashFatTarget">0</span> g</div>
          </div>
        </div>

        <div class="divider"></div>

        <div class="row cols2">
          <div class="kpi">
            <div class="cap">Calories balance</div>
            <div class="big" id="dashCalBalance">0 kcal</div>
            <div class="cap" id="dashCalBalanceMsg">—</div>
          </div>
          <div class="kpi">
            <div class="cap">Protein balance</div>
            <div class="big" id="dashProtBalance">0 g</div>
            <div class="cap" id="dashProtBalanceMsg">—</div>
          </div>
        </div>

        <div class="divider"></div>

        <div class="row cols3">
          <div class="pill">BMI: <b id="dashBMI">—</b></div>
          <div class="pill">BMR: <b id="dashBMR">—</b> kcal</div>
          <div class="pill">TDEE: <b id="dashTDEE">—</b> kcal</div>
        </div>

        <div style="margin-top:10px" class="sub">
          Recommended BMI range: <b>18.5–24.9</b>. Your BMI marker is shown below (color-coded).
        </div>

        <div style="margin-top:10px">
          <div class="bmiBar"><div class="bmiFill" id="bmiFill"></div></div>
          <div class="bmiMarker">
            <span style="left:18.5%">18.5</span>
            <span style="left:24.9%">24.9</span>
            <span id="bmiPin" style="left:0%">▲ You</span>
          </div>
        </div>

        <div class="note" id="profileSavedNote" style="display:none;">
          Profile saved ✔️ You won’t need to re-enter your details on this device.
        </div>

        <div class="divider"></div>

        <button class="btn" onclick="savePDF()">Save PDF Report</button>
      </div>

      <div class="card">
        <h2>Macro Chart</h2>
        <div class="sub">Fixed-height chart (won’t “run away” while scrolling). Colors are distinct and NOT blue.</div>
        <div class="chartBox">
          <canvas id="macroChart"></canvas>
        </div>
      </div>
    </section>

    <!-- PROFILE -->
    <section id="page-profile" class="grid cols2 hide" style="margin-top:14px;">
      <div class="card">
        <h2>Profile</h2>
        <div class="sub">Default Height unit is Feet/Inches (mobile friendly). Save once.</div>

        <div class="divider"></div>

        <div class="row cols2">
          <div>
            <label>Name</label>
            <input id="p_name" placeholder="Enter your name">
          </div>
          <div>
            <label>Age</label>
            <input type="number" min="1" step="1" id="p_age" placeholder="Enter your age">
          </div>
        </div>

        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Height unit</label>
            <select id="p_height_unit">
              <option value="ftin" selected>Feet/Inches</option>
              <option value="cm">Centimeters (cm)</option>
            </select>
          </div>
          <div>
            <label>Weight unit</label>
            <select id="p_weight_unit">
              <option value="kg" selected>Kilograms (kg)</option>
              <option value="lbs">Pounds (lbs)</option>
            </select>
          </div>
        </div>

        <div class="row cols3 hide" style="margin-top:10px" id="height_cm_row">
          <div>
            <label>Height (cm)</label>
            <input type="number" min="0" step="0.1" id="p_height_cm" placeholder="e.g., 175">
          </div>
          <div class="hide"></div>
          <div class="hide"></div>
        </div>

        <div class="row cols3" style="margin-top:10px" id="height_ftin_row">
          <div>
            <label>Height (ft)</label>
            <input type="number" min="0" step="1" id="p_height_ft" placeholder="e.g., 5">
          </div>
          <div>
            <label>Height (in)</label>
            <input type="number" min="0" step="1" id="p_height_in" placeholder="e.g., 9">
          </div>
          <div class="hide"></div>
        </div>

        <div class="row cols2" style="margin-top:10px">
          <div id="weight_kg_row">
            <label>Weight (kg)</label>
            <input type="number" min="0" step="0.1" id="p_weight_kg" placeholder="e.g., 75">
          </div>
          <div id="weight_lbs_row" class="hide">
            <label>Weight (lbs)</label>
            <input type="number" min="0" step="0.1" id="p_weight_lbs" placeholder="e.g., 165">
          </div>
        </div>

        <div class="divider"></div>

        <div class="row cols2">
          <div>
            <label>Goal (default: weight loss)</label>
            <select id="p_goal">
              <option value="loss" selected>Weight loss</option>
              <option value="maintain">Maintenance</option>
              <option value="gain">Weight gain</option>
            </select>
          </div>
          <div>
            <label>Activity level</label>
            <select id="p_activity">
              <option value="1.2">Sedentary — little or no exercise</option>
              <option value="1.375">Light — exercise 1–3 times/week</option>
              <option value="1.55">Moderate — exercise 4–5 times/week</option>
              <option value="1.725">Active — daily exercise or intense 3–4 times/week</option>
              <option value="1.9">Very Active — intense exercise 6–7 times/week</option>
              <option value="2.0">Extra Active — very intense daily or physical job</option>
            </select>
          </div>
        </div>

        <div class="row cols3" style="margin-top:10px">
          <div>
            <label>Day starts at (hour)</label>
            <input type="number" min="0" max="23" step="1" id="p_dayStartHour" value="4">
          </div>
          <div>
            <label>Sex (for BMR)</label>
            <select id="p_sex">
              <option value="male" selected>Male</option>
              <option value="female">Female</option>
            </select>
          </div>
          <div>
            <label>Macro style</label>
            <select id="p_macroPreset">
              <option value="balanced" selected>Balanced</option>
              <option value="higherCarb">Higher carb (bulking friendly)</option>
              <option value="higherProtein">Higher protein</option>
            </select>
          </div>
        </div>

        <div class="divider"></div>

        <h3>Targets (auto-filled, editable)</h3>
        <div class="row cols3" style="margin-top:10px">
          <div>
            <label>Target calories</label>
            <input type="number" min="0" step="1" id="p_targetCalories" placeholder="Auto">
          </div>
          <div>
            <label>Target protein (g)</label>
            <input type="number" min="0" step="1" id="p_targetProtein" placeholder="Auto">
          </div>
          <div>
            <label>Target carbs (g)</label>
            <input type="number" min="0" step="1" id="p_targetCarbs" placeholder="Auto">
          </div>
        </div>

        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Target fats (g)</label>
            <input type="number" min="0" step="1" id="p_targetFats" placeholder="Auto">
          </div>
          <div>
            <label>Protein multiplier (g/kg)</label>
            <select id="p_protMult">
              <option value="1.5">1.5 g/kg</option>
              <option value="1.6" selected>1.6 g/kg</option>
              <option value="1.8">1.8 g/kg</option>
              <option value="2.0">2.0 g/kg</option>
            </select>
          </div>
        </div>

        <div class="divider"></div>

        <button class="btn" onclick="saveProfile()">Save Profile</button>

        <div class="note" id="profileSaveHint" style="display:none;">
          Saved ✔️ Go to Food Log and add entries (lunch, dinner, snacks, etc.).
        </div>
      </div>

      <div class="card">
        <h2>Metrics (BMI • BMR • TDEE)</h2>
        <div class="row cols3">
          <div class="kpi"><div class="cap">BMI</div><div class="big" id="p_bmiLine">—</div><div class="cap">Recommended 18.5–24.9</div></div>
          <div class="kpi"><div class="cap">BMR</div><div class="big" id="p_bmrLine">—</div><div class="cap">Mifflin–St Jeor</div></div>
          <div class="kpi"><div class="cap">TDEE</div><div class="big" id="p_tdeeLine">—</div><div class="cap" id="p_targetLine">Target: —</div></div>
        </div>
        <div class="divider"></div>
        <div class="sub">
          Tip: Save profile once. Food log entries can be added multiple times per day.
        </div>
      </div>
    </section>

    <!-- FOOD LOG -->
    <section id="page-food" class="grid cols2 hide" style="margin-top:14px;">
      <div class="card">
        <h2>Food Log (Multiple entries/day)</h2>
        <div class="sub">Pick a food → qty → meal type → Add Entry. Repeat as many times as you want.</div>

        <div class="divider"></div>

        <div class="row cols2">
          <div>
            <label>Log date</label>
            <input type="date" id="logDate">
          </div>
          <div>
            <label>Meal type</label>
            <select id="mealType">
              <option>Breakfast</option>
              <option>Brunch</option>
              <option selected>Lunch</option>
              <option>Dinner</option>
              <option>Supper</option>
              <option>Pre-workout meal</option>
              <option>Post-workout meal</option>
              <option>Snacks</option>
            </select>
          </div>
        </div>

        <div class="divider"></div>

        <h3>Add Food Entry</h3>
        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Food item</label>
            <select id="entryFood"></select>
          </div>
          <div>
            <label>Quantity</label>
            <input type="number" min="0" step="1" id="entryQty" placeholder="Enter quantity (g / tbsp / pcs / plate)">
          </div>
          <div>
            <label>Unit</label>
            <select id="entryUnit"></select>
          </div>
          <div>
            <label>&nbsp;</label>
            <button class="btn" onclick="addEntry()">Add Entry</button>
          </div>
        </div>

        <div class="pill" style="margin-top:10px">
          Instant preview: <b><span id="entryPrevP">0</span>g P</b> • <b><span id="entryPrevF">0</span>g F</b> • <b><span id="entryPrevC">0</span>g C</b> • <b><span id="entryPrevK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <div class="row cols2">
          <button class="btn" onclick="saveDay()">Save for the Day</button>
          <button class="btn danger" onclick="resetFoodLogs()">Reset Food Logs</button>
        </div>

        <div class="divider"></div>

        <h3>Daily Entries</h3>
        <div class="sub">You can edit quantity inline or delete any row.</div>
        <div class="tableWrap" style="margin-top:10px">
          <table>
            <thead>
              <tr>
                <th>Meal</th>
                <th>Food</th>
                <th>Qty</th>
                <th>Unit</th>
                <th>P (g)</th>
                <th>C (g)</th>
                <th>F (g)</th>
                <th>Kcal</th>
                <th>Action</th>
              </tr>
            </thead>
            <tbody id="entriesTbody"></tbody>
          </table>
        </div>
      </div>

      <div class="card">
        <h2>Live Totals (Auto)</h2>
        <div class="sub">Totals are based on the entries table for the selected day.</div>

        <div class="divider"></div>

        <div class="row cols2">
          <div class="kpi"><div class="cap">Protein</div><div class="big" id="liveP">0 g</div></div>
          <div class="kpi"><div class="cap">Carbs</div><div class="big" id="liveC">0 g</div></div>
          <div class="kpi"><div class="cap">Fat</div><div class="big" id="liveF">0 g</div></div>
          <div class="kpi"><div class="cap">Calories</div><div class="big" id="liveK">0 kcal</div></div>
        </div>

        <div class="divider"></div>

        <div class="sub">
          Key “reliable” items: Butter 1 tbsp (14g) ~102 kcal, Olive oil 1 tbsp ~119 kcal, Soybean oil 1 tbsp ~120 kcal, Ghee 1 tbsp ~126 kcal, Peanut butter 1 tbsp ~100 kcal (label-based).
        </div>
      </div>
    </section>
  </div>

<script>
/* =========================
   Storage keys
========================= */
const LS_PROFILE = "nowshad_macro_profile_v4";
const LS_LOG = "nowshad_macro_dailylog_v4";

/* =========================
   Helpers
========================= */
const $ = (id)=>document.getElementById(id);
const clamp = (v,min,max)=>Math.max(min,Math.min(max,v));
const n = (v)=>isFinite(parseFloat(v)) ? parseFloat(v) : 0;
const round1 = (x)=>Math.round(x*10)/10;
const round2 = (x)=>Math.round(x*100)/100;
function sum(a,b){ return {P:a.P+b.P, C:a.C+b.C, F:a.F+b.F, K:a.K+b.K}; }
function zero(){ return {P:0,C:0,F:0,K:0}; }

/* =========================
   Tabs
========================= */
document.querySelectorAll(".tabbtn").forEach(btn=>{
  btn.addEventListener("click", ()=>{
    document.querySelectorAll(".tabbtn").forEach(b=>b.classList.remove("active"));
    btn.classList.add("active");
    showPage(btn.dataset.page);
  });
});
function showPage(name){
  ["dashboard","profile","food"].forEach(p=>{
    $("page-"+p).classList.toggle("hide", p!==name);
  });
}

/* =========================
   Food DB (keeps your earlier style)
========================= */
const FOOD = {
  // unit: g, pcs, tbsp, plate, scoop, slice, cup, ml, serv
  "Chicken Breast": { unitOptions:["g"], perUnit:{g:{P:31/100, C:0, F:3.6/100, K:165/100}} },
  "Chicken Thigh": { unitOptions:["g"], perUnit:{g:{P:26/100, C:0, F:8/100, K:209/100}} },
  "Chicken Wings": { unitOptions:["g"], perUnit:{g:{P:30/100, C:0, F:13/100, K:290/100}} },
  "Chicken Drumsticks": { unitOptions:["g"], perUnit:{g:{P:28/100, C:0, F:10/100, K:215/100}} },

  "Egg": { unitOptions:["pcs"], perUnit:{pcs:{P:6, C:0.6, F:5, K:72}} },
  "Protein Shake": { unitOptions:["scoop"], perUnit:{scoop:{P:25, C:3, F:2, K:120}} },

  "Mutton": { unitOptions:["g"], perUnit:{g:{P:25/100, C:0, F:20/100, K:294/100}} },
  "Duck": { unitOptions:["g"], perUnit:{g:{P:23/100, C:0, F:28/100, K:337/100}} },
  "Pigeon": { unitOptions:["g"], perUnit:{g:{P:25/100, C:0, F:15/100, K:250/100}} },
  "Quail": { unitOptions:["g"], perUnit:{g:{P:22/100, C:0, F:12/100, K:210/100}} },
  "Beef": { unitOptions:["g"], perUnit:{g:{P:26/100, C:0, F:15/100, K:250/100}} },
  "Fish": { unitOptions:["g"], perUnit:{g:{P:22/100, C:0, F:5/100, K:120/100}} },
  "Shrimp": { unitOptions:["g"], perUnit:{g:{P:24/100, C:0, F:2/100, K:99/100}} },
  "Dried Fish / Bombay Duck": { unitOptions:["g"], perUnit:{g:{P:60/100, C:0, F:5/100, K:300/100}} },

  "Rice (cooked)": { unitOptions:["g","plate"], perUnit:{
      g:{P:2.7/100, C:28/100, F:0.3/100, K:130/100},
      plate:{P:8, C:90, F:1, K:420} // approx
  }},
  "Bread": { unitOptions:["slice"], perUnit:{slice:{P:3, C:15, F:1, K:80}} },
  "Roti": { unitOptions:["pcs"], perUnit:{pcs:{P:3.5, C:18, F:3, K:120}} },
  "Paratha": { unitOptions:["pcs"], perUnit:{pcs:{P:6, C:30, F:12, K:260}} },
  "Oats (dry)": { unitOptions:["g"], perUnit:{g:{P:16.9/100, C:66.3/100, F:6.9/100, K:389/100}} },

  "Milk Tea": { unitOptions:["cup"], perUnit:{cup:{P:2, C:10, F:3, K:80}} },
  "Carbonated Beverage": { unitOptions:["ml"], perUnit:{ml:{P:0, C:10.6/100, F:0, K:42/100}} },

  "Salad": { unitOptions:["cup"], perUnit:{cup:{P:0.5, C:2, F:0.1, K:15}} },
  "Vegetables": { unitOptions:["cup"], perUnit:{cup:{P:2, C:5, F:0.2, K:35}} },

  // Fats (reliable-ish)
  "Butter": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0.12, C:0.01, F:11.5, K:102}} },
  "Olive Oil": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0, C:0, F:13.5, K:119}} },
  "Soybean Oil": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0, C:0, F:13.6, K:120}} },
  "Ghee": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0, C:0, F:14, K:126}} },

  // Snacks & Fruits
  "Peanut Butter": { unitOptions:["tbsp"], perUnit:{tbsp:{P:3.5, C:4, F:8, K:100}} },
  "Yogurt / Curd": { unitOptions:["g"], perUnit:{g:{P:3.5/100, C:4.7/100, F:3.3/100, K:61/100}} },

  "Dates (Deglet Noor style)": { unitOptions:["g","pcs"], perUnit:{
      g:{P:2.2/100, C:75/100, F:0.3/100, K:282/100},
      pcs:{P:0.18, C:6.0, F:0.02, K:23} // ~ 8g date
  }},
  "Dates (Ajwa small)": { unitOptions:["pcs"], perUnit:{pcs:{P:0.20, C:6.7, F:0.03, K:26}} }, // ~9g/date scaled
  "Dates (Medjool large)": { unitOptions:["pcs"], perUnit:{pcs:{P:0.43, C:18, F:0.05, K:66}} }, // large date approx

  "Almonds": { unitOptions:["g"], perUnit:{g:{P:21.2/100, C:21.6/100, F:49.9/100, K:579/100}} },
  "Peanuts": { unitOptions:["g"], perUnit:{g:{P:25.8/100, C:16.1/100, F:49.2/100, K:567/100}} },
  "Cashews": { unitOptions:["g"], perUnit:{g:{P:18.2/100, C:30.2/100, F:43.9/100, K:553/100}} },
  "Walnuts": { unitOptions:["g"], perUnit:{g:{P:15.2/100, C:13.7/100, F:65.2/100, K:654/100}} },
  "Pistachios": { unitOptions:["g"], perUnit:{g:{P:20.2/100, C:27.2/100, F:45.3/100, K:562/100}} },

  "Banana": { unitOptions:["pcs"], perUnit:{pcs:{P:1.3, C:27, F:0.4, K:105}} },
  "Apple": { unitOptions:["pcs"], perUnit:{pcs:{P:0.5, C:25, F:0.3, K:95}} },
  "Orange": { unitOptions:["pcs"], perUnit:{pcs:{P:1.2, C:15.4, F:0.2, K:62}} },
  "Mango (1 cup slices)": { unitOptions:["cup"], perUnit:{cup:{P:1.4, C:25, F:0.6, K:99}} },
  "Grapes (1 cup)": { unitOptions:["cup"], perUnit:{cup:{P:1.1, C:27.3, F:0.2, K:104}} },

  // Meals (plate)
  "Kacchi Biryani": { unitOptions:["plate"], perUnit:{plate:{P:26, C:90, F:39, K:800}} },
  "Biryani (general)": { unitOptions:["plate"], perUnit:{plate:{P:20, C:85, F:15, K:600}} },
  "Khichuri": { unitOptions:["plate"], perUnit:{plate:{P:14, C:70, F:12, K:450}} },
  "Fried Rice": { unitOptions:["plate"], perUnit:{plate:{P:12, C:75, F:14, K:520}} },
  "Shawarma (wrap)": { unitOptions:["pcs"], perUnit:{pcs:{P:30, C:40, F:20, K:450}} },
  "Chicken Broast": { unitOptions:["plate"], perUnit:{plate:{P:35, C:25, F:25, K:550}} },
  "Beef Steak Meal": { unitOptions:["plate"], perUnit:{plate:{P:45, C:10, F:25, K:500}} },
  "Fried Chicken Meal (KFC style)": { unitOptions:["plate"], perUnit:{plate:{P:35, C:45, F:30, K:700}} },
  "Whopper": { unitOptions:["pcs"], perUnit:{pcs:{P:30.4, C:51.1, F:38.4, K:678.8}} },
  "Double Whopper": { unitOptions:["pcs"], perUnit:{pcs:{P:46, C:46, F:52, K:838}} },
};

const FOOD_ORDER = [
  { group:"Chicken", items:["Chicken Breast","Chicken Thigh","Chicken Wings","Chicken Drumsticks"] },
  { group:"Other protein sources", items:["Egg","Protein Shake","Mutton","Duck","Pigeon","Quail","Beef","Fish","Shrimp","Dried Fish / Bombay Duck"] },
  { group:"Carbs", items:["Rice (cooked)","Bread","Roti","Paratha","Oats (dry)","Milk Tea","Carbonated Beverage"] },
  { group:"Vegetables & Salad", items:["Salad","Vegetables"] },
  { group:"Fats", items:["Butter","Olive Oil","Soybean Oil","Ghee"] },
  { group:"Snacks & Fruits", items:["Peanut Butter","Yogurt / Curd","Dates (Deglet Noor style)","Dates (Ajwa small)","Dates (Medjool large)","Almonds","Peanuts","Cashews","Walnuts","Pistachios","Banana","Apple","Orange","Mango (1 cup slices)","Grapes (1 cup)"] },
  { group:"Meals", items:["Kacchi Biryani","Biryani (general)","Khichuri","Fried Rice","Shawarma (wrap)","Chicken Broast","Beef Steak Meal","Fried Chicken Meal (KFC style)","Whopper","Double Whopper"] },
];

/* =========================
   Chart (no blue)
========================= */
let macroChart=null;
function initChart(){
  const ctx=$("macroChart").getContext("2d");
  macroChart=new Chart(ctx,{
    type:"bar",
    data:{
      labels:["Protein (g)","Carbs (g)","Fat (g)"],
      datasets:[{label:"Macros", data:[0,0,0], backgroundColor:["#3ddc97","#ffcc66","#ff6b6b"]}]
    },
    options:{
      responsive:true, maintainAspectRatio:false, animation:false,
      scales:{
        y:{beginAtZero:true, ticks:{color:"rgba(238,243,255,.8)"}, grid:{color:"rgba(255,255,255,.10)"}},
        x:{ticks:{color:"rgba(238,243,255,.8)"}, grid:{display:false}}
      },
      plugins:{ legend:{labels:{color:"rgba(238,243,255,.85)"}} }
    }
  });
}
initChart();

/* =========================
   Profile calc
========================= */
function getProfileDraft(){
  const sex=$("p_sex").value;
  const age=n($("p_age").value);
  const wUnit=$("p_weight_unit").value;
  const hUnit=$("p_height_unit").value;

  let wkg = wUnit==="kg" ? n($("p_weight_kg").value) : n($("p_weight_lbs").value)*0.45359237;
  let hm=0;
  if(hUnit==="cm") hm=n($("p_height_cm").value)/100;
  else hm=(n($("p_height_ft").value)*0.3048)+(n($("p_height_in").value)*0.0254);
  const hcm=hm*100;

  const bmi=(wkg>0&&hm>0)?(wkg/(hm*hm)):0;
  let bmr=0;
  if(wkg>0&&hcm>0&&age>0){
    bmr=(10*wkg)+(6.25*hcm)-(5*age)+(sex==="male"?5:-161);
  }
  const activity=n($("p_activity").value);
  const tdee=bmr?bmr*activity:0;

  const goal=$("p_goal").value;
  let targetCalories=tdee;
  if(goal==="loss") targetCalories=tdee-500;
  if(goal==="gain") targetCalories=tdee+400;

  const pMin=wkg? wkg*1.5 : 0;
  const pMax=wkg? wkg*2.0 : 0;
  let pTargetAuto=0;
  if(pMin>0){
    const ref=pMin+(pMax-pMin)*0.45;
    pTargetAuto=Math.round(ref/5)*5;
  }

  // preset split
  const preset=$("p_macroPreset").value;
  let fatPct=.30, carbPct=.40;
  if(preset==="higherCarb"){ fatPct=.25; carbPct=.50; }
  if(preset==="higherProtein"){ fatPct=.25; carbPct=.35; }

  let pTarget = n($("p_targetProtein").value) || pTargetAuto;
  let userTC=n($("p_targetCalories").value);
  if(userTC>0) targetCalories=userTC;

  const remaining=Math.max(0, targetCalories - pTarget*4);
  const cfTotal=carbPct+fatPct;
  const cTargetAuto=Math.round(((remaining*(carbPct/cfTotal))/4)/5)*5;
  const fTargetAuto=Math.round((remaining*(fatPct/cfTotal))/9);

  const cTarget=n($("p_targetCarbs").value) || cTargetAuto;
  const fTarget=n($("p_targetFats").value) || fTargetAuto;

  return {
    name:$("p_name").value||"",
    sex, age,
    heightUnit:hUnit, weightUnit:wUnit,
    height_cm:n($("p_height_cm").value),
    height_ft:n($("p_height_ft").value),
    height_in:n($("p_height_in").value),
    weight_kg:n($("p_weight_kg").value),
    weight_lbs:n($("p_weight_lbs").value),
    wkg, hm, bmi, bmr, tdee,
    goal, activity,
    dayStartHour:clamp(parseInt($("p_dayStartHour").value||"4",10),0,23),
    targetCalories, targetProtein:pTarget, targetCarbs:cTarget, targetFats:fTarget,
    macroPreset:preset
  };
}

function updateProfilePreview(){
  const p=getProfileDraft();
  $("p_bmiLine").textContent = p.bmi? round2(p.bmi) : "—";
  $("p_bmrLine").textContent = p.bmr? Math.round(p.bmr)+" kcal" : "—";
  $("p_tdeeLine").textContent = p.tdee? Math.round(p.tdee)+" kcal" : "—";
  $("p_targetLine").textContent = "Target: " + (p.targetCalories?Math.round(p.targetCalories)+" kcal":"—");

  if(!n($("p_targetCalories").value) && p.targetCalories) $("p_targetCalories").value=Math.round(p.targetCalories);
  if(!n($("p_targetProtein").value) && p.targetProtein) $("p_targetProtein").value=p.targetProtein;
  if(!n($("p_targetCarbs").value) && p.targetCarbs) $("p_targetCarbs").value=p.targetCarbs;
  if(!n($("p_targetFats").value) && p.targetFats) $("p_targetFats").value=p.targetFats;

  refreshAll();
}

function saveProfile(){
  const p=getProfileDraft();
  localStorage.setItem(LS_PROFILE, JSON.stringify(p));
  $("profileSaveHint").style.display="block";
  $("profileSavedNote").style.display="block";
  refreshAll();
}

function loadProfile(){
  const raw=localStorage.getItem(LS_PROFILE);
  if(!raw) return null;
  try{
    const p=JSON.parse(raw);
    $("p_name").value=p.name||"";
    $("p_age").value=p.age||"";
    $("p_sex").value=p.sex||"male";

    $("p_height_unit").value=p.heightUnit||"ftin";
    $("p_weight_unit").value=p.weightUnit||"kg";

    $("p_height_cm").value=p.height_cm||"";
    $("p_height_ft").value=p.height_ft||"";
    $("p_height_in").value=p.height_in||"";

    $("p_weight_kg").value=p.weight_kg||"";
    $("p_weight_lbs").value=p.weight_lbs||"";

    $("p_goal").value=p.goal||"loss";
    $("p_activity").value=(p.activity||1.2).toString();
    $("p_dayStartHour").value=(p.dayStartHour ?? 4);
    $("p_macroPreset").value=p.macroPreset||"balanced";

    $("p_targetCalories").value=p.targetCalories?Math.round(p.targetCalories):"";
    $("p_targetProtein").value=p.targetProtein||"";
    $("p_targetCarbs").value=p.targetCarbs||"";
    $("p_targetFats").value=p.targetFats||"";

    $("profileSavedNote").style.display="block";
    toggleHeightUI();
    toggleWeightUI();
    return p;
  }catch(e){ return null; }
}

/* Height/Weight toggles */
function toggleHeightUI(){
  const u=$("p_height_unit").value;
  $("height_cm_row").classList.toggle("hide", u!=="cm");
  $("height_ftin_row").classList.toggle("hide", u!=="ftin");
  updateProfilePreview();
}
function toggleWeightUI(){
  const u=$("p_weight_unit").value;
  $("weight_kg_row").classList.toggle("hide", u!=="kg");
  $("weight_lbs_row").classList.toggle("hide", u!=="lbs");
  updateProfilePreview();
}
$("p_height_unit").addEventListener("change", toggleHeightUI);
$("p_weight_unit").addEventListener("change", toggleWeightUI);

/* Profile listeners */
[
 "p_name","p_age","p_height_cm","p_height_ft","p_height_in","p_weight_kg","p_weight_lbs",
 "p_goal","p_activity","p_dayStartHour","p_sex","p_macroPreset",
 "p_targetCalories","p_targetProtein","p_targetCarbs","p_targetFats"
].forEach(id=>{
  $(id).addEventListener("input", updateProfilePreview);
  $(id).addEventListener("change", updateProfilePreview);
});

/* =========================
   Day key
========================= */
function getDefaultLogDate(profile){
  const now=new Date();
  const dayStart=profile?.dayStartHour ?? 4;
  const adjusted=new Date(now);
  if(now.getHours()<dayStart) adjusted.setDate(now.getDate()-1);
  return adjusted.toISOString().slice(0,10);
}

/* =========================
   Entries model
========================= */
function loadAllLogs(){
  try{ return JSON.parse(localStorage.getItem(LS_LOG) || "{}"); }catch(e){ return {}; }
}
function saveAllLogs(obj){ localStorage.setItem(LS_LOG, JSON.stringify(obj)); }
function getDayLog(dateKey){
  const all=loadAllLogs();
  return all[dateKey] || { entries: [] };
}
function setDayLog(dateKey, dayLog){
  const all=loadAllLogs();
  all[dateKey]=dayLog;
  saveAllLogs(all);
}

/* =========================
   Entry compute
========================= */
function computeFood(foodName, qty, unit){
  const item=FOOD[foodName];
  if(!item) return zero();
  const map=item.perUnit[unit];
  if(!map) return zero();
  // map is per 1 unit, but for g/ml we stored per 1g or per 1ml already
  return {
    P: map.P*qty,
    C: map.C*qty,
    F: map.F*qty,
    K: map.K*qty
  };
}

/* =========================
   Food entry UI
========================= */
function buildFoodSelect(){
  const sel=$("entryFood");
  sel.innerHTML="";
  FOOD_ORDER.forEach(group=>{
    const og=document.createElement("optgroup");
    og.label=group.group;
    group.items.forEach(name=>{
      const opt=document.createElement("option");
      opt.value=name; opt.textContent=name;
      og.appendChild(opt);
    });
    sel.appendChild(og);
  });
}
function buildUnitSelect(foodName){
  const uSel=$("entryUnit");
  uSel.innerHTML="";
  (FOOD[foodName]?.unitOptions || ["g"]).forEach(u=>{
    const opt=document.createElement("option");
    opt.value=u;
    opt.textContent = (u==="pcs"?"pcs":u);
    uSel.appendChild(opt);
  });
}
function updateEntryPreview(){
  const food=$("entryFood").value;
  const unit=$("entryUnit").value;
  const qty=n($("entryQty").value);
  const t=computeFood(food, qty, unit);
  $("entryPrevP").textContent=round1(t.P);
  $("entryPrevC").textContent=round1(t.C);
  $("entryPrevF").textContent=round1(t.F);
  $("entryPrevK").textContent=Math.round(t.K);
}
function addEntry(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  const day=getDayLog(dateKey);

  const food=$("entryFood").value;
  const unit=$("entryUnit").value;
  const qty=n($("entryQty").value);
  if(!food || qty<=0){ alert("Select a food and enter a quantity > 0."); return; }

  day.entries.push({
    id: crypto.randomUUID ? crypto.randomUUID() : String(Date.now())+Math.random(),
    meal: $("mealType").value,
    food, unit, qty
  });
  setDayLog(dateKey, day);

  $("entryQty").value="";
  renderEntries();
  refreshAll();
}

/* Render entries table */
function renderEntries(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  const day=getDayLog(dateKey);

  const tb=$("entriesTbody");
  tb.innerHTML="";

  day.entries.forEach((e, idx)=>{
    const t=computeFood(e.food, n(e.qty), e.unit);
    const tr=document.createElement("tr");
    tr.innerHTML=`
      <td>${e.meal}</td>
      <td>${e.food}</td>
      <td><input class="miniInput" type="number" min="0" step="1" value="${e.qty}" data-id="${e.id}" data-field="qty"></td>
      <td>${e.unit}</td>
      <td>${round1(t.P)}</td>
      <td>${round1(t.C)}</td>
      <td>${round1(t.F)}</td>
      <td>${Math.round(t.K)}</td>
      <td><button class="actionBtn" data-id="${e.id}" data-action="del">Delete</button></td>
    `;
    tb.appendChild(tr);
  });

  // Inline edit qty
  tb.querySelectorAll('input[data-field="qty"]').forEach(inp=>{
    inp.addEventListener("input", ()=>{
      const id=inp.dataset.id;
      const day2=getDayLog(dateKey);
      const row=day2.entries.find(x=>x.id===id);
      if(row){ row.qty = n(inp.value); setDayLog(dateKey, day2); renderEntries(); refreshAll(); }
    });
  });
  // Delete
  tb.querySelectorAll('button[data-action="del"]').forEach(btn=>{
    btn.addEventListener("click", ()=>{
      const id=btn.dataset.id;
      const day2=getDayLog(dateKey);
      day2.entries = day2.entries.filter(x=>x.id!==id);
      setDayLog(dateKey, day2);
      renderEntries();
      refreshAll();
    });
  });
}

/* Save/Reset buttons you requested */
function saveDay(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  // data is already in storage as you add/edit/delete
  alert("Saved for the Day ✔️ ("+dateKey+")");
}
function resetFoodLogs(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  if(!confirm("Reset all food entries for "+dateKey+"?")) return;
  setDayLog(dateKey, { entries: [] });
  renderEntries();
  refreshAll();
}

/* =========================
   Totals from entries
========================= */
function computeTotalsFromEntries(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  const day=getDayLog(dateKey);

  let total=zero();
  day.entries.forEach(e=>{
    total = sum(total, computeFood(e.food, n(e.qty), e.unit));
  });
  return total;
}

/* =========================
   Dashboard refresh
========================= */
function updateBMIBar(bmi){
  const fill=$("bmiFill");
  const pin=$("bmiPin");
  if(!bmi || bmi<=0){ fill.style.width="0%"; pin.style.left="0%"; pin.textContent="▲ You"; return; }
  const pct=clamp(((bmi-10)/(40-10))*100, 0, 100);
  fill.style.width=pct+"%";
  pin.style.left=pct+"%";
  pin.textContent="▲ You ("+round1(bmi)+")";
}

function refreshAll(){
  const profile=loadProfile() || getProfileDraft();

  // Ensure date
  if(!$("logDate").value) $("logDate").value = getDefaultLogDate(profile);

  // Render entries
  renderEntries();

  // Totals
  const total=computeTotalsFromEntries();

  // Live totals
  $("liveP").textContent = round1(total.P) + " g";
  $("liveC").textContent = round1(total.C) + " g";
  $("liveF").textContent = round1(total.F) + " g";
  $("liveK").textContent = Math.round(total.K) + " kcal";

  // Dashboard KPIs
  $("dashCalories").textContent = Math.round(total.K) + " kcal";
  $("dashProtein").textContent = round1(total.P) + " g";
  $("dashCarbs").textContent = round1(total.C);
  $("dashFat").textContent = round1(total.F);

  const tCal=n(profile.targetCalories);
  const tP=n(profile.targetProtein);
  const tC=n(profile.targetCarbs);
  const tF=n(profile.targetFats);

  $("dashCalTarget").textContent = tCal?Math.round(tCal):0;
  $("dashProtTarget").textContent = tP?tP:0;
  $("dashCarbTarget").textContent = tC?tC:0;
  $("dashFatTarget").textContent = tF?tF:0;

  const calDiff=Math.round(total.K - tCal);
  const protDiff=Math.round(total.P - tP);

  if(!tCal){
    $("dashCalBalance").textContent="—";
    $("dashCalBalanceMsg").textContent="Set profile targets first.";
    $("dashCalBalance").className="big";
  }else{
    if(calDiff>0){
      $("dashCalBalance").textContent="+"+calDiff+" kcal";
      $("dashCalBalance").className="big accentBad";
      $("dashCalBalanceMsg").textContent="Over target (you ate more than target).";
    }else if(calDiff<0){
      $("dashCalBalance").textContent=calDiff+" kcal";
      $("dashCalBalance").className="big accentGood";
      $("dashCalBalanceMsg").textContent="Under target (you ate less than target).";
    }else{
      $("dashCalBalance").textContent="0 kcal";
      $("dashCalBalance").className="big";
      $("dashCalBalanceMsg").textContent="Exactly on target.";
    }
  }

  if(!tP){
    $("dashProtBalance").textContent="—";
    $("dashProtBalanceMsg").textContent="Set protein target in Profile.";
    $("dashProtBalance").className="big";
  }else{
    if(protDiff>0){
      $("dashProtBalance").textContent="+"+protDiff+" g";
      $("dashProtBalance").className="big accentGood";
      $("dashProtBalanceMsg").textContent="Above target protein.";
    }else if(protDiff<0){
      $("dashProtBalance").textContent=protDiff+" g";
      $("dashProtBalance").className="big accentBad";
      $("dashProtBalanceMsg").textContent="Below target protein.";
    }else{
      $("dashProtBalance").textContent="0 g";
      $("dashProtBalance").className="big";
      $("dashProtBalanceMsg").textContent="Exactly on target protein.";
    }
  }

  // Metrics
  $("dashBMI").textContent = profile.bmi ? round2(profile.bmi) : "—";
  $("dashBMR").textContent = profile.bmr ? Math.round(profile.bmr) : "—";
  $("dashTDEE").textContent = profile.tdee ? Math.round(profile.tdee) : "—";
  updateBMIBar(profile.bmi);

  // Chart
  if(macroChart){
    macroChart.data.datasets[0].data = [round1(total.P), round1(total.C), round1(total.F)];
    macroChart.update();
  }
}

/* =========================
   PDF (uses totals from entries)
========================= */
async function savePDF(){
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ unit:"pt", format:"a4" });
  const profile = loadProfile() || getProfileDraft();
  const total = computeTotalsFromEntries();

  const pageW = doc.internal.pageSize.getWidth();
  let y=46;

  doc.setFont("helvetica","bold"); doc.setFontSize(18);
  doc.text("Nowshad's Macro Calculator — Report", 40, y); y+=18;

  doc.setFont("helvetica","normal"); doc.setFontSize(10);
  doc.text("Generated: " + new Date().toLocaleString(), 40, y); y+=20;

  doc.setFont("helvetica","bold"); doc.setFontSize(12);
  doc.text("Profile Summary", 40, y); y+=10;
  doc.setDrawColor(220); doc.setLineWidth(1); doc.line(40,y,pageW-40,y); y+=12;

  doc.setFont("helvetica","normal"); doc.setFontSize(11);
  const lines = [
    `Name: ${profile.name || "—"}   |   Age: ${profile.age || "—"}   |   Sex: ${profile.sex || "—"}`,
    `BMI: ${profile.bmi ? round2(profile.bmi) : "—"} (Recommended 18.5–24.9)   |   BMR: ${profile.bmr ? Math.round(profile.bmr) : "—"} kcal   |   TDEE: ${profile.tdee ? Math.round(profile.tdee) : "—"} kcal`,
    `Goal: ${profile.goal || "—"}   |   Target Calories: ${profile.targetCalories ? Math.round(profile.targetCalories) : "—"} kcal`,
    `Targets — Protein: ${profile.targetProtein || "—"} g   |   Carbs: ${profile.targetCarbs || "—"} g   |   Fats: ${profile.targetFats || "—"} g`,
  ];
  lines.forEach(t=>{ doc.text(t,40,y); y+=16; });

  y+=8;
  doc.setFont("helvetica","bold"); doc.text("Today’s Totals", 40, y); y+=10;
  doc.setFont("helvetica","normal");
  doc.text(`Calories: ${Math.round(total.K)} kcal   |   Protein: ${round1(total.P)} g   |   Carbs: ${round1(total.C)} g   |   Fat: ${round1(total.F)} g`, 40, y);
  y+=18;

  // Chart image
  doc.setFont("helvetica","bold"); doc.text("Macro Chart", 40, y); y+=10;
  const canvas = $("macroChart");
  const imgData = canvas.toDataURL("image/png", 1.0);
  doc.addImage(imgData, "PNG", 40, y, pageW-80, 220);
  y+=235;

  doc.setFontSize(9);
  doc.text("Disclaimer: Nutrition values are estimates. For personalized advice consult a qualified nutritionist/doctor.", 40, y);

  doc.save("Nowshad_Macro_Report.pdf");
}

/* =========================
   Boot + entry selector init
========================= */
function bootEntrySelector(){
  buildFoodSelect();
  const first = $("entryFood").value;
  buildUnitSelect(first);
  $("entryFood").addEventListener("change", ()=>{
    buildUnitSelect($("entryFood").value);
    updateEntryPreview();
  });
  $("entryUnit").addEventListener("change", updateEntryPreview);
  $("entryQty").addEventListener("input", updateEntryPreview);
  updateEntryPreview();
}

/* =========================
   Init
========================= */
(function init(){
  loadProfile();

  // Force default height unit ft/in on first load (if profile doesn't exist)
  if(!localStorage.getItem(LS_PROFILE)){
    $("p_height_unit").value="ftin";
    toggleHeightUI();
  }else{
    toggleHeightUI();
    toggleWeightUI();
  }

  updateProfilePreview();

  // Default log date
  const p = loadProfile() || getProfileDraft();
  $("logDate").value = getDefaultLogDate(p);

  $("logDate").addEventListener("change", ()=>{ renderEntries(); refreshAll(); });

  bootEntrySelector();
  refreshAll();
})();
</script>

</body>
</html>
