
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Nowshad's Macro Calculator</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">

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
    .brand h1{font-size:16px; margin:0; letter-spacing:.2px; font-weight:800}
    .brand span{font-size:12px; color:var(--muted)}
    .tabs{display:flex; gap:8px; flex-wrap:wrap; justify-content:flex-end}
    .tabbtn{
      padding:10px 12px; border-radius:12px;
      border:1px solid var(--stroke); background:rgba(255,255,255,.08);
      color:var(--text); cursor:pointer; font-weight:700; font-size:13px;
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
    }
    label{font-size:12px; color:var(--muted); display:block; margin-bottom:6px}
    input, select{
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
    input[readonly]{opacity:.95}
    .btn{
      padding:11px 12px; border-radius:12px; border:1px solid var(--stroke);
      background:rgba(255,255,255,.14); color:var(--text);
      cursor:pointer; font-weight:800;
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
    th{color:rgba(238,243,255,.75); font-weight:800; background:rgba(255,255,255,.06)}
    .miniInput{
      width:90px; padding:8px 8px; border-radius:10px; font-size:12px;
    }
    .actionBtn{
      padding:8px 10px; border-radius:10px; border:1px solid rgba(255,255,255,.18);
      background:rgba(255,255,255,.10); color:var(--text); cursor:pointer; font-weight:800; font-size:12px;
    }
    .actionBtn:hover{background:rgba(255,255,255,.14)}
  </style>
</head>

<body>
  <div class="wrap">
    <div class="topbar">
      <div class="brand">
        <h1>Nowshad's Macro Calculator</h1>
        <span>Profile → Food Log → Lifestyle → Dashboard + PDF</span>
      </div>
      <div class="tabs">
        <button class="tabbtn active" data-page="dashboard">Dashboard</button>
        <button class="tabbtn" data-page="profile">Profile</button>
        <button class="tabbtn" data-page="food">Food Log</button>
        <button class="tabbtn" data-page="lifestyle">Lifestyle</button>
      </div>
    </div>

    <!-- DASHBOARD -->
    <section id="page-dashboard" class="grid cols2" style="margin-top:14px;">
      <div class="card">
        <h2>Today’s Dashboard</h2>
        <div class="sub">Targets are based on your Profile. Protein target is always weight × multiplier. Carbs target is calories left after protein + fats.</div>

        <div class="divider"></div>

        <div class="row cols3">
          <div class="kpi">
            <div class="cap">Calories eaten</div>
            <div class="big" id="dashCalories">0 kcal</div>
            <div class="cap">Target: <span id="dashCalTarget">0</span> kcal</div>
          </div>
          <div class="kpi">
            <div class="cap">Workout burned</div>
            <div class="big" id="dashBurn">0 kcal</div>
            <div class="cap">Net: <span id="dashNet">0</span> kcal</div>
          </div>
          <div class="kpi">
            <div class="cap">Protein eaten</div>
            <div class="big" id="dashProtein">0 g</div>
            <div class="cap">Target: <span id="dashProtTarget">0</span> g</div>
          </div>
        </div>

        <div class="row cols3" style="margin-top:10px">
          <div class="kpi">
            <div class="cap">Carbs eaten</div>
            <div class="big" id="dashCarbs">0 g</div>
            <div class="cap">Target: <span id="dashCarbTarget">0</span> g</div>
          </div>
          <div class="kpi">
            <div class="cap">Fat eaten</div>
            <div class="big" id="dashFat">0 g</div>
            <div class="cap">Target: <span id="dashFatTarget">0</span> g</div>
          </div>
          <div class="kpi">
            <div class="cap">Net vs Target (calories)</div>
            <div class="big" id="dashCalBalance">—</div>
            <div class="cap" id="dashCalBalanceMsg">Set profile targets first.</div>
          </div>
        </div>

        <div class="divider"></div>

        <div class="row cols3">
          <div class="kpi">
            <div class="cap">Protein balance</div>
            <div class="big" id="dashProtBalance">—</div>
            <div class="cap" id="dashProtBalanceMsg">—</div>
          </div>
          <div class="kpi">
            <div class="cap">Carbs balance</div>
            <div class="big" id="dashCarbBalance">—</div>
            <div class="cap" id="dashCarbBalanceMsg">—</div>
          </div>
          <div class="kpi">
            <div class="cap">Fat balance</div>
            <div class="big" id="dashFatBalance">—</div>
            <div class="cap" id="dashFatBalanceMsg">—</div>
          </div>
        </div>

        <div class="divider"></div>

        <div class="row cols3">
          <div class="pill">BMI: <b id="dashBMI">—</b></div>
          <div class="pill">BMR: <b id="dashBMR">—</b> kcal</div>
          <div class="pill">TDEE: <b id="dashTDEE">—</b> kcal</div>
        </div>

        <div style="margin-top:10px" class="sub">
          Recommended BMI range: <b>18.5–24.9</b>. Your BMI marker is shown below.
        </div>

        <div style="margin-top:10px">
          <div class="bmiBar"><div class="bmiFill" id="bmiFill"></div></div>
          <div class="bmiMarker">
            <span style="left:18.5%">18.5</span>
            <span style="left:24.9%">24.9</span>
            <span id="bmiPin" style="left:0%">▲ You</span>
          </div>
        </div>

        <div class="divider"></div>

        <div class="row cols3">
          <div class="kpi">
            <div class="cap">Water</div>
            <div class="big" id="dashWater">0 L</div>
            <div class="cap">Goal: <span id="dashWaterGoal">0</span> L</div>
          </div>
          <div class="kpi">
            <div class="cap">Sleep</div>
            <div class="big" id="dashSleep">0 h</div>
            <div class="cap">Goal: <span id="dashSleepGoal">0</span> h</div>
          </div>
          <div class="kpi">
            <div class="cap">Lifestyle notes</div>
            <div class="cap" id="dashLifeMsg">Update in Lifestyle tab</div>
          </div>
        </div>

        <div class="note" id="profileSavedNote" style="display:none;">
          Profile saved ✔️ You won’t need to re-enter your details on this device.
        </div>

        <div class="divider"></div>

       <div class="row cols2">

<div class="row cols3">
  <button class="btn" onclick="savePDF()">Save PDF Report</button>
  <button class="btn secondary" onclick="resetToday()">Reset Only Today</button>
  <button class="btn danger" onclick="resetEverything()">Reset Everything</button>
</div>

  <button class="btn danger" onclick="resetEverything()">Reset Everything</button>
</div>

      </div>

      <div class="card">
        <h2>Macro Chart</h2>
        <div class="sub">Stable chart on scroll. Colors are distinct and NOT blue.</div>
        <div class="chartBox">
          <canvas id="macroChart"></canvas>
        </div>
      </div>

      <div class="divider"></div>

<h3>Profiles</h3>
<div class="sub">Switch users on the same device. Each profile has its own Profile + Food logs + Lifestyle.</div>

<div class="row cols2" style="margin-top:10px;">
  <div>
    <label>Active profile</label>
    <select id="profileSelect"></select>
  </div>
  <div>
    <label>New profile name</label>
    <input id="newProfileName" placeholder="e.g., Nowshad / Aymaan / Guest">
  </div>
</div>

<div class="row cols3" style="margin-top:10px;">
  <button class="btn" onclick="createProfile()">Create Profile</button>
  <button class="btn secondary" onclick="resetToday()">Reset Only Today</button>
  <button class="btn danger" onclick="deleteActiveProfile()">Delete Profile</button>
</div>

    </section>

    <!-- PROFILE -->
    <section id="page-profile" class="grid cols2 hide" style="margin-top:14px;">
      <div class="card">
        <h2>Profile</h2>
        <div class="sub">Feet/Inches is default. Target Calories is based on TDEE + goal. Target Carbs is calculated from remaining calories after Protein + Fats.</div>

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

        <div class="row cols2 hide" style="margin-top:10px" id="height_cm_row">
          <div>
            <label>Height (cm)</label>
            <input type="number" min="0" step="0.1" id="p_height_cm" placeholder="e.g., 175">
          </div>
          <div class="hide"></div>
        </div>

        <div class="row cols2" style="margin-top:10px" id="height_ftin_row">
          <div>
            <label>Height (ft)</label>
            <input type="number" min="0" step="1" id="p_height_ft" placeholder="e.g., 5">
          </div>
          <div>
            <label>Height (in)</label>
            <input type="number" min="0" step="1" id="p_height_in" placeholder="e.g., 9">
          </div>
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
            <label>Macro style (default: Higher protein)</label>
            <select id="p_macroPreset">
              <option value="balanced">Balanced</option>
              <option value="higherCarb">Higher carb (bulking friendly)</option>
              <option value="higherProtein" selected>Higher protein</option>
            </select>
          </div>
        </div>

        <div class="divider"></div>

        <h3>Targets</h3>
        <div class="sub">Protein target is always weight × multiplier. If you edit Target Calories/Fats/Carbs, your edit will be kept.</div>

        <div class="row cols3" style="margin-top:10px">
          <div>
            <label>Target calories (auto from TDEE, editable)</label>
            <input type="number" min="0" step="1" id="p_targetCalories" placeholder="Auto">
          </div>
          <div>
            <label>Target protein (g) — auto from weight</label>
            <input type="number" id="p_targetProtein" readonly>
          </div>
          <div>
            <label>Target fats (g) (auto, editable)</label>
            <input type="number" min="0" step="1" id="p_targetFats" placeholder="Auto">
          </div>
        </div>

        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Target carbs (g) (calculated from remaining calories, editable)</label>
            <input type="number" min="0" step="1" id="p_targetCarbs" placeholder="Auto">
          </div>
          <div>
            <label>Protein multiplier (g/kg) — default 2.2</label>
            <select id="p_protMult">
              <option value="1.5">1.5 g/kg</option>
              <option value="1.6">1.6 g/kg</option>
              <option value="1.8">1.8 g/kg</option>
              <option value="2.0">2.0 g/kg</option>
              <option value="2.2" selected>2.2 g/kg</option>
            </select>
          </div>
        </div>

        <div class="divider"></div>

       <div class="row cols2" style="margin-top:10px">
  <button class="btn" onclick="saveProfile()">Save Profile</button>
  <button class="btn danger" onclick="resetProfile()">Reset Profile Data</button>
</div>

function resetProfile(){
  if(!confirm("Reset profile data on this device? This will remove saved user profile/targets so you can enter a new user.")) return;

  // Remove saved profile
  localStorage.removeItem(LS_PROFILE);

  // Clear dirty flags (so auto-fill works again)
  ["p_targetCalories","p_targetFats","p_targetCarbs"].forEach(id=>{
    setDirty(id,false);
  });

  // Clear form fields
  $("p_name").value="";
  $("p_age").value="";
  $("p_sex").value="male";

  $("p_height_unit").value="ftin";
  $("p_weight_unit").value="kg";

  $("p_height_cm").value="";
  $("p_height_ft").value="";
  $("p_height_in").value="";
  $("p_weight_kg").value="";
  $("p_weight_lbs").value="";

  $("p_goal").value="loss";
  $("p_activity").value="1.2";
  $("p_dayStartHour").value="4";
  $("p_macroPreset").value="higherProtein";
  $("p_protMult").value="2.2";

  // Clear targets so they re-auto-calc from the new profile
  $("p_targetCalories").value="";
  $("p_targetProtein").value="";
  $("p_targetFats").value="";
  $("p_targetCarbs").value="";

  // Hide saved notes
  $("profileSaveHint").style.display="none";
  $("profileSavedNote").style.display="none";

  // Rebuild UI + refresh dashboard
  toggleHeightUI(false);
  toggleWeightUI(false);
  updateProfilePreviewOnly();
  scheduleRefresh();

  alert("Profile data reset ✔️ Now enter the new user details and Save Profile.");
}

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
        <div class="sub">TDEE = BMR × Activity Level. Target Calories = TDEE + goal adjustment.</div>
      </div>
    </section>

    <!-- FOOD LOG -->
    <section id="page-food" class="grid cols2 hide" style="margin-top:14px;">
      <div class="card">
        <h2>Food Log (Multiple entries/day)</h2>
        <div class="sub">Choose Category → Food → Unit → Qty → Add Entry (repeat anytime).</div>

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
            <label>Category</label>
            <select id="entryCategory"></select>
          </div>
          <div>
            <label>Food item</label>
            <select id="entryFood"></select>
          </div>
          <div>
            <label>Unit</label>
            <select id="entryUnit"></select>
          </div>
          <div>
            <label>Quantity</label>
            <input type="number" min="0" step="1" id="entryQty" placeholder="Enter qty">
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
        <div class="sub">Edit quantity inline or delete any row.</div>
        <div class="tableWrap" style="margin-top:10px">
          <table>
            <thead>
              <tr>
                <th>Meal</th>
                <th>Category</th>
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
      </div>
    </section>

    <!-- LIFESTYLE -->
    <section id="page-lifestyle" class="grid cols2 hide" style="margin-top:14px;">
      <div class="card">
        <h2>Lifestyle (Sleep • Water • Workout)</h2>
        <div class="sub">Saved per day. Workout calories burned reduce your net calories on Dashboard.</div>

        <div class="divider"></div>

        <div class="row cols2">
          <div>
            <label>Date</label>
            <input type="date" id="lifeDate">
          </div>
          <div>
            <label>Workout type</label>
            <select id="workoutType">
              <option value="none" selected>None</option>
              <option value="walking">Walking</option>
              <option value="running">Running</option>
              <option value="cycling">Cycling</option>
              <option value="aerobic">Aerobics</option>
              <option value="cardio">Cardio (general)</option>
              <option value="hiit">HIIT</option>
              <option value="strength">Strength training</option>
            </select>
          </div>
        </div>

        <div class="row cols2" style="margin-top:10px">
          <div id="strengthSplitWrap" class="hide">
            <label>Strength split</label>
            <select id="strengthSplit">
              <option value="chest_triceps">Chest + Triceps</option>
              <option value="back_biceps">Back + Biceps</option>
              <option value="shoulders_forearms">Shoulders + Forearms</option>
              <option value="legs">Legs</option>
              <option value="fullbody">Full body</option>
            </select>
          </div>
          <div>
            <label>Workout duration (minutes)</label>
            <input type="number" min="0" step="1" id="workoutMins" placeholder="e.g., 45">
          </div>
        </div>

        <div class="pill" style="margin-top:10px">
          Estimated calories burned: <b><span id="burnPreview">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <h3>Sleep</h3>
        <div class="row cols2">
          <div>
            <label>Hours slept</label>
            <input type="number" min="0" step="0.1" id="sleepHours" placeholder="e.g., 7.5">
          </div>
          <div>
            <label>Sleep goal (hours)</label>
            <select id="sleepGoal">
              <option value="7">7</option>
              <option value="8" selected>8</option>
              <option value="9">9</option>
            </select>
          </div>
        </div>

        <div class="divider"></div>

        <h3>Water</h3>
        <div class="row cols2">
          <div>
            <label>Water intake (liters)</label>
            <input type="number" min="0" step="0.1" id="waterLiters" placeholder="e.g., 2.5">
          </div>
          <div>
            <label>Water goal (liters)</label>
            <select id="waterGoal">
              <option value="2">2.0</option>
              <option value="2.5">2.5</option>
              <option value="3" selected>3.0</option>
              <option value="3.5">3.5</option>
              <option value="4">4.0</option>
            </select>
          </div>
        </div>

        <div class="divider"></div>

        <div class="row cols2">
          <button class="btn" onclick="saveLifestyle()">Save Lifestyle</button>
          <button class="btn danger" onclick="resetLifestyle()">Reset Lifestyle</button>
        </div>

        <div class="note" id="lifeSavedNote" style="display:none;">
          Lifestyle saved ✔️ Dashboard updated.
        </div>
      </div>

      <div class="card">
        <h2>Today’s Lifestyle Summary</h2>
        <div class="divider"></div>

        <div class="row cols2">
          <div class="kpi">
            <div class="cap">Workout</div>
            <div class="big" id="lifeWorkoutLine">—</div>
            <div class="cap">Minutes • Estimated burn</div>
          </div>
          <div class="kpi">
            <div class="cap">Sleep / Water</div>
            <div class="big" id="lifeSleepWaterLine">—</div>
            <div class="cap">Today’s lifestyle summary</div>
          </div>
        </div>
      </div>
    </section>
  </div>

<script>
/* ===== Storage ===== */
const LS_PROFILE = "nowshad_macro_profile_v9";
const LS_LOG = "nowshad_macro_dailylog_v9";
const LS_LIFE = "nowshad_macro_lifestyle_v9";
/* ===== Multi-profile layer ===== */
const LS_ACTIVE_PROFILE = "nowshad_active_profile_v1";
const LS_PROFILE_LIST   = "nowshad_profile_list_v1";

function getActiveProfileId(){
  return localStorage.getItem(LS_ACTIVE_PROFILE) || "default";
}
function setActiveProfileId(id){
  localStorage.setItem(LS_ACTIVE_PROFILE, id);
}
function getProfileList(){
  try{
    const arr = JSON.parse(localStorage.getItem(LS_PROFILE_LIST) || "[]");
    return Array.isArray(arr) && arr.length ? arr : [{id:"default", name:"Default"}];
  }catch(e){
    return [{id:"default", name:"Default"}];
  }
}
function saveProfileList(list){
  localStorage.setItem(LS_PROFILE_LIST, JSON.stringify(list));
}

function keyFor(baseKey){
  // Namespace all data by active profile
  return `${baseKey}__${getActiveProfileId()}`;
}

function ensureProfileSystem(){
  let list = getProfileList();
  if(!list.find(x=>x.id==="default")){
    list.unshift({id:"default", name:"Default"});
  }
  saveProfileList(list);

  const active = getActiveProfileId();
  if(!list.find(x=>x.id===active)){
    setActiveProfileId("default");
  }
}

function rebuildProfileSelect(){
  const sel = document.getElementById("profileSelect");
  if(!sel) return;

  const list = getProfileList();
  const active = getActiveProfileId();

  sel.innerHTML = "";
  list.forEach(p=>{
    const opt = document.createElement("option");
    opt.value = p.id;
    opt.textContent = p.name;
    sel.appendChild(opt);
  });
  sel.value = active;

  sel.onchange = ()=>{
    setActiveProfileId(sel.value);

    // Re-load everything for selected profile
    loadProfile();                 // hydrates profile UI
    updateProfilePreviewOnly();
    updateLifestyleUI();

    // Set date defaults for this profile
    const p = loadProfile() || getProfileDraft();
    $("logDate").value = getDefaultLogDate(p);
    $("lifeDate").value = getDefaultLogDate(p);

    renderEntries();
    refreshAll();
  };
}

function createProfile(){
  const name = (document.getElementById("newProfileName")?.value || "").trim();
  if(!name){ alert("Enter a profile name."); return; }

  let list = getProfileList();
  const id = "p_" + Date.now().toString(36) + Math.random().toString(36).slice(2,7);

  list.push({id, name});
  saveProfileList(list);

  setActiveProfileId(id);

  // Start clean for new profile
  localStorage.removeItem(keyFor(LS_PROFILE));
  localStorage.removeItem(keyFor(LS_LOG));
  localStorage.removeItem(keyFor(LS_LIFE));

  if(document.getElementById("newProfileName")) document.getElementById("newProfileName").value = "";

  rebuildProfileSelect();

  // Clear UI immediately
  resetEverything(true); // silent internal reset for new profile
}

function deleteActiveProfile(){
  const active = getActiveProfileId();
  if(active === "default"){
    alert("Default profile cannot be deleted. You can reset it instead.");
    return;
  }
  const list = getProfileList();
  const current = list.find(x=>x.id===active);
  if(!confirm(`Delete profile "${current?.name || "this profile"}" and ALL its data?`)) return;

  // Delete data for that profile
  localStorage.removeItem(`${LS_PROFILE}__${active}`);
  localStorage.removeItem(`${LS_LOG}__${active}`);
  localStorage.removeItem(`${LS_LIFE}__${active}`);

  // Remove from list and switch to default
  const newList = list.filter(x=>x.id!==active);
  saveProfileList(newList);
  setActiveProfileId("default");

  rebuildProfileSelect();
  resetEverything(true); // reload default cleanly (will keep default data wiped due to true)
}

/* ===== Helpers ===== */
const $ = (id)=>document.getElementById(id);
const clamp = (v,min,max)=>Math.max(min,Math.min(max,v));
const n = (v)=>isFinite(parseFloat(v)) ? parseFloat(v) : 0;
const round1 = (x)=>Math.round(x*10)/10;
const round2 = (x)=>Math.round(x*100)/100;
function sum(a,b){ return {P:a.P+b.P, C:a.C+b.C, F:a.F+b.F, K:a.K+b.K}; }
function zero(){ return {P:0,C:0,F:0,K:0}; }

/* ===== Tabs ===== */
document.querySelectorAll(".tabbtn").forEach(btn=>{
  btn.addEventListener("click", ()=>{
    document.querySelectorAll(".tabbtn").forEach(b=>b.classList.remove("active"));
    btn.classList.add("active");
    showPage(btn.dataset.page);
    scheduleRefresh();
  });
});
function showPage(name){
  ["dashboard","profile","food","lifestyle"].forEach(p=>{
    $("page-"+p).classList.toggle("hide", p!==name);
  });
}

/* ===== Chart (no blue) ===== */
let macroChart=null;
(function initChart(){
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
})();

/* ===== Debounced refresh ===== */
let refreshTimer=null;
function scheduleRefresh(){
  if(refreshTimer) clearTimeout(refreshTimer);
  refreshTimer=setTimeout(()=>{ refreshAll(); }, 160);
}

/* ===== Unit-safe readers + auto-convert on switch ===== */
function readWeightKg(){
  const u = $("p_weight_unit").value;
  const kg = n($("p_weight_kg").value);
  const lbs = n($("p_weight_lbs").value);
  return (u==="lbs") ? lbs*0.45359237 : kg;
}
function readHeightCm(){
  const u = $("p_height_unit").value;
  const cm = n($("p_height_cm").value);
  const ft = n($("p_height_ft").value);
  const inch = n($("p_height_in").value);
  return (u==="cm") ? cm : (ft*30.48 + inch*2.54);
}
function convertWeight(from,to){
  const kg = (from==="kg") ? n($("p_weight_kg").value) : n($("p_weight_lbs").value)*0.45359237;
  if(!kg) return;
  if(to==="kg"){ $("p_weight_kg").value = round1(kg); $("p_weight_lbs").value=""; }
  else { $("p_weight_lbs").value = round1(kg/0.45359237); $("p_weight_kg").value=""; }
}
function convertHeight(from,to){
  let cm = 0;
  if(from==="cm"){ cm = n($("p_height_cm").value); }
  else { cm = n($("p_height_ft").value)*30.48 + n($("p_height_in").value)*2.54; }
  if(!cm) return;
  if(to==="cm"){ $("p_height_cm").value = round1(cm); $("p_height_ft").value=""; $("p_height_in").value=""; }
  else {
    const totalIn = cm/2.54;
    const ft = Math.floor(totalIn/12);
    const inch = Math.round(totalIn - ft*12);
    $("p_height_ft").value = ft;
    $("p_height_in").value = inch;
    $("p_height_cm").value="";
  }
}

/* ===== Targets dirty flags (saved) ===== */
function setDirty(id, val=true){ $(id).dataset.dirty = val ? "1" : ""; }
function isDirty(id){ return $(id).dataset.dirty === "1"; }
["p_targetCalories","p_targetFats","p_targetCarbs"].forEach(id=>{
  $(id).addEventListener("input", ()=>setDirty(id,true));
});

/* ===== Profile calc (CALORIES from TDEE; CARBS from remaining after P+F) ===== */
function getProfileDraft(){
  const sex=$("p_sex").value;
  const age=n($("p_age").value);

  const wkg = readWeightKg();
  const hcm = readHeightCm();
  const hm = hcm/100;

  const bmi=(wkg>0&&hm>0)?(wkg/(hm*hm)):0;

  let bmr=0;
  if(wkg>0 && hcm>0 && age>0){
    bmr = (10*wkg) + (6.25*hcm) - (5*age) + (sex==="male"?5:-161);
  }

  const activity=n($("p_activity").value);
  const tdee=bmr?bmr*activity:0;
  const basicsOk = (wkg>0 && hcm>0 && age>0 && tdee>0);

  const protMult = n($("p_protMult").value) || 2.2;
  const targetProtein = (wkg>0) ? Math.round((wkg * protMult)/5)*5 : 0;

  let targetCaloriesAuto = 0;
  if (basicsOk) {
    const goal = $("p_goal").value;
    const adj = goal === "gain" ? 400 : goal === "loss" ? -500 : 0;
    targetCaloriesAuto = Math.round(tdee + adj);
  }

  const targetCalories = (isDirty("p_targetCalories") && n($("p_targetCalories").value)>0)
    ? Math.max(1200, Math.round(n($("p_targetCalories").value)))
    : targetCaloriesAuto;

  let fatPct = 0.30;
  const preset=$("p_macroPreset").value;
  if(preset==="higherCarb") fatPct = 0.25;
  if(preset==="higherProtein") fatPct = 0.25;

  let fatsAuto = 0;
  if(targetCalories>0){
    fatsAuto = Math.round((targetCalories * fatPct)/9);
  }
  let targetFats = (isDirty("p_targetFats") && n($("p_targetFats").value)>0)
    ? Math.max(0, Math.round(n($("p_targetFats").value)))
    : fatsAuto;

  let carbsAuto = 0;
  if(targetCalories>0 && targetProtein>0){
    let remaining = targetCalories - (targetProtein*4) - (targetFats*9);
    if(remaining < 0){
      targetFats = Math.max(0, Math.floor((targetCalories - targetProtein*4)/9));
      remaining = Math.max(0, targetCalories - (targetProtein*4) - (targetFats*9));
    }
    carbsAuto = Math.max(0, Math.round(remaining/4));
  }
  const targetCarbs = (isDirty("p_targetCarbs") && n($("p_targetCarbs").value)>=0)
    ? Math.max(0, Math.round(n($("p_targetCarbs").value)))
    : carbsAuto;

  return {
    name:$("p_name").value||"",
    sex, age,
    heightUnit:$("p_height_unit").value,
    weightUnit:$("p_weight_unit").value,
    height_cm:n($("p_height_cm").value),
    height_ft:n($("p_height_ft").value),
    height_in:n($("p_height_in").value),
    weight_kg:n($("p_weight_kg").value),
    weight_lbs:n($("p_weight_lbs").value),
    wkg, hm, bmi, bmr, tdee,
    goal:$("p_goal").value,
    activity,
    dayStartHour:clamp(parseInt($("p_dayStartHour").value||"4",10),0,23),
    macroPreset:$("p_macroPreset").value,
    protMult,
    targetCalories,
    targetProtein,
    targetFats,
    targetCarbs,
    dirtyCalories:isDirty("p_targetCalories"),
    dirtyFats:isDirty("p_targetFats"),
    dirtyCarbs:isDirty("p_targetCarbs"),
  };
}

/* ===== Profile UI updates ===== */
function toggleHeightUI(doRecalc=true){
  const u=$("p_height_unit").value;
  $("height_cm_row").classList.toggle("hide", u!=="cm");
  $("height_ftin_row").classList.toggle("hide", u!=="ftin");
  if(doRecalc) updateProfilePreviewOnly();
}
function toggleWeightUI(doRecalc=true){
  const u=$("p_weight_unit").value;
  $("weight_kg_row").classList.toggle("hide", u!=="kg");
  $("weight_lbs_row").classList.toggle("hide", u!=="lbs");
  if(doRecalc) updateProfilePreviewOnly();
}

$("p_height_unit").addEventListener("change", ()=>{
  const to = $("p_height_unit").value;
  convertHeight(to==="cm" ? "ftin" : "cm", to);
  toggleHeightUI(true);
});
$("p_weight_unit").addEventListener("change", ()=>{
  const to = $("p_weight_unit").value;
  convertWeight(to==="kg" ? "lbs" : "kg", to);
  toggleWeightUI(true);
});

function updateProfilePreviewOnly(){
  const p=getProfileDraft();

  $("p_bmiLine").textContent = p.bmi? round2(p.bmi) : "—";
  $("p_bmrLine").textContent = p.bmr? Math.round(p.bmr)+" kcal" : "—";
  $("p_tdeeLine").textContent = p.tdee? Math.round(p.tdee)+" kcal" : "—";
  $("p_targetLine").textContent = "Target: " + (p.targetCalories?Math.round(p.targetCalories)+" kcal":"—");

  $("p_targetProtein").value = p.targetProtein ? p.targetProtein : "";

  if(!isDirty("p_targetCalories") && (!n($("p_targetCalories").value)) && p.targetCalories) $("p_targetCalories").value = p.targetCalories;
  if(!isDirty("p_targetFats") && (!n($("p_targetFats").value)) && p.targetFats>=0) $("p_targetFats").value = p.targetFats;
  if(!isDirty("p_targetCarbs") && (!n($("p_targetCarbs").value)) && p.targetCarbs>=0) $("p_targetCarbs").value = p.targetCarbs;

  scheduleRefresh();
}

function saveProfile(){
  try{
    const p=getProfileDraft();
   localStorage.setItem(keyFor(LS_PROFILE), JSON.stringify(p));

    $("profileSaveHint").style.display="block";
    $("profileSavedNote").style.display="block";
    scheduleRefresh();
  }catch(e){
    alert("Could not save profile. Your browser storage may be restricted.");
  }
}

function loadProfile(){
  const raw=localStorage.getItem(keyFor(LS_PROFILE));

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
    $("p_macroPreset").value=p.macroPreset||"higherProtein";
    $("p_protMult").value=(p.protMult || 2.2).toString();

    setDirty("p_targetCalories", !!p.dirtyCalories);
    setDirty("p_targetFats", !!p.dirtyFats);
    setDirty("p_targetCarbs", !!p.dirtyCarbs);

    $("p_targetCalories").value = p.dirtyCalories && p.targetCalories ? Math.round(p.targetCalories) : "";
    $("p_targetFats").value = p.dirtyFats && (p.targetFats>=0) ? Math.round(p.targetFats) : "";
    $("p_targetCarbs").value = p.dirtyCarbs && (p.targetCarbs>=0) ? Math.round(p.targetCarbs) : "";

    $("profileSavedNote").style.display="block";
    toggleHeightUI(false);
    toggleWeightUI(false);
    updateProfilePreviewOnly();
    return p;
  }catch(e){ return null; }
}

/* Profile listeners */
[
 "p_name","p_age","p_height_cm","p_height_ft","p_height_in","p_weight_kg","p_weight_lbs",
 "p_goal","p_activity","p_dayStartHour","p_sex","p_macroPreset","p_protMult",
 "p_targetCalories","p_targetFats","p_targetCarbs"
].forEach(id=>{
  $(id).addEventListener("input", updateProfilePreviewOnly);
  $(id).addEventListener("change", updateProfilePreviewOnly);
});

/* ===== Day key ===== */
function getDefaultLogDate(profile){
  const now=new Date();
  const dayStart=profile?.dayStartHour ?? 4;
  const adjusted=new Date(now);
  if(now.getHours()<dayStart) adjusted.setDate(now.getDate()-1);
  return adjusted.toISOString().slice(0,10);
}

/* ===== Logs ===== */
function loadAllLogs(){ 
  try{ return JSON.parse(localStorage.getItem(keyFor(LS_LOG)) || "{}"); }
  catch(e){ return {}; }
}
function saveAllLogs(obj){ localStorage.setItem(keyFor(LS_LOG), JSON.stringify(obj)); }

function getDayLog(dateKey){ const all=loadAllLogs(); return all[dateKey] || { entries: [] }; }
function setDayLog(dateKey, dayLog){ const all=loadAllLogs(); all[dateKey]=dayLog; saveAllLogs(all); }

/* ===== Food DB ===== */
const FOOD = {
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
      plate:{P:8, C:90, F:1, K:420}
  }},
  "Bread": { unitOptions:["slice"], perUnit:{slice:{P:3, C:15, F:1, K:80}} },
  "Roti": { unitOptions:["pcs"], perUnit:{pcs:{P:3.5, C:18, F:3, K:120}} },
  "Paratha": { unitOptions:["pcs"], perUnit:{pcs:{P:6, C:30, F:12, K:260}} },
  "Oats (dry)": { unitOptions:["g"], perUnit:{g:{P:16.9/100, C:66.3/100, F:6.9/100, K:389/100}} },
  "Milk Tea": { unitOptions:["cup"], perUnit:{cup:{P:2, C:10, F:3, K:80}} },
  "Carbonated Beverage": { unitOptions:["ml"], perUnit:{ml:{P:0, C:10.6/100, F:0, K:42/100}} },

  "Salad": { unitOptions:["cup"], perUnit:{cup:{P:0.5, C:2, F:0.1, K:15}} },
  "Vegetables": { unitOptions:["cup"], perUnit:{cup:{P:2, C:5, F:0.2, K:35}} },

  "Butter": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0.12, C:0.01, F:11.5, K:102}} },
  "Olive Oil": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0, C:0, F:13.5, K:119}} },
  "Soybean Oil": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0, C:0, F:13.6, K:120}} },
  "Ghee": { unitOptions:["tbsp"], perUnit:{tbsp:{P:0, C:0, F:14, K:126}} },

  "Peanut Butter": { unitOptions:["tbsp"], perUnit:{tbsp:{P:3.5, C:4, F:8, K:100}} },
  "Yogurt / Curd": { unitOptions:["g"], perUnit:{g:{P:3.5/100, C:4.7/100, F:3.3/100, K:61/100}} },

  "Dates (Deglet Noor style)": { unitOptions:["g","pcs"], perUnit:{
      g:{P:2.2/100, C:75/100, F:0.3/100, K:282/100},
      pcs:{P:0.18, C:6.0, F:0.02, K:23}
  }},
  "Dates (Ajwa small)": { unitOptions:["pcs"], perUnit:{pcs:{P:0.20, C:6.7, F:0.03, K:26}} },
  "Dates (Medjool large)": { unitOptions:["pcs"], perUnit:{pcs:{P:0.43, C:18, F:0.05, K:66}} },

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

  /* ===== Dessert (NEW) ===== */
  "Cake Slice": { unitOptions:["slice"], perUnit:{slice:{P:4, C:45, F:12, K:300}} },
  "Chocolate Cube": { unitOptions:["cube"], perUnit:{cube:{P:1, C:6, F:3, K:55}} },
  "Ice Cream Scoop": { unitOptions:["scoop"], perUnit:{scoop:{P:2.5, C:16, F:7.5, K:140}} },
  "Rice Pudding (Kheer)": { unitOptions:["bowl"], perUnit:{bowl:{P:6, C:34, F:7, K:220}} },
  "Shemai (Semiya)": { unitOptions:["bowl"], perUnit:{bowl:{P:7, C:52, F:10, K:330}} },
  "Jorda": { unitOptions:["plate"], perUnit:{plate:{P:6, C:70, F:12, K:420}} },
  "Firni": { unitOptions:["bowl"], perUnit:{bowl:{P:6, C:36, F:8, K:240}} },
};

const CATEGORY_ITEMS = {
  "Chicken": ["Chicken Breast","Chicken Thigh","Chicken Wings","Chicken Drumsticks"],
  "Protein": ["Egg","Protein Shake","Mutton","Duck","Pigeon","Quail","Beef","Fish","Shrimp","Dried Fish / Bombay Duck"],
  "Carbs": ["Rice (cooked)","Bread","Roti","Paratha","Oats (dry)","Milk Tea","Carbonated Beverage"],
  "Vegetables": ["Salad","Vegetables"],
  "Fats": ["Butter","Olive Oil","Soybean Oil","Ghee"],
  "Snacks & Fruits": ["Peanut Butter","Yogurt / Curd","Dates (Deglet Noor style)","Dates (Ajwa small)","Dates (Medjool large)","Almonds","Peanuts","Cashews","Walnuts","Pistachios","Banana","Apple","Orange","Mango (1 cup slices)","Grapes (1 cup)"],
  "Meals": ["Kacchi Biryani","Biryani (general)","Khichuri","Fried Rice","Shawarma (wrap)","Chicken Broast","Beef Steak Meal","Fried Chicken Meal (KFC style)","Whopper","Double Whopper"],
  "Dessert": ["Cake Slice","Chocolate Cube","Ice Cream Scoop","Rice Pudding (Kheer)","Shemai (Semiya)","Jorda","Firni"]
};

/* ===== Entry compute ===== */
function computeFood(foodName, qty, unit){
  const item=FOOD[foodName];
  if(!item) return zero();
  const map=item.perUnit[unit];
  if(!map) return zero();
  return { P: map.P*qty, C: map.C*qty, F: map.F*qty, K: map.K*qty };
}

/* ===== Food UI ===== */
function buildCategorySelect(){
  const sel=$("entryCategory");
  sel.innerHTML="";
  Object.keys(CATEGORY_ITEMS).forEach(cat=>{
    const opt=document.createElement("option");
    opt.value=cat; opt.textContent=cat;
    sel.appendChild(opt);
  });
}
function buildFoodSelectForCategory(category){
  const sel=$("entryFood");
  sel.innerHTML="";
  (CATEGORY_ITEMS[category]||[]).forEach(name=>{
    const opt=document.createElement("option");
    opt.value=name; opt.textContent=name;
    sel.appendChild(opt);
  });
}
function buildUnitSelect(foodName){
  const uSel=$("entryUnit");
  uSel.innerHTML="";
  (FOOD[foodName]?.unitOptions || ["g"]).forEach(u=>{
    const opt=document.createElement("option");
    opt.value=u; opt.textContent=u;
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
$("entryCategory").addEventListener("change", ()=>{
  buildFoodSelectForCategory($("entryCategory").value);
  buildUnitSelect($("entryFood").value);
  updateEntryPreview();
});
$("entryFood").addEventListener("change", ()=>{
  buildUnitSelect($("entryFood").value);
  updateEntryPreview();
});
$("entryUnit").addEventListener("change", updateEntryPreview);
$("entryQty").addEventListener("input", updateEntryPreview);

function computeTotalsFromEntries(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  const day=getDayLog(dateKey);
  let total=zero();
  day.entries.forEach(e=> total = sum(total, computeFood(e.food, n(e.qty), e.unit)));
  return total;
}

/* ===== Add entry ===== */
function addEntry(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  const day=getDayLog(dateKey);

  const category=$("entryCategory").value;
  const food=$("entryFood").value;
  const unit=$("entryUnit").value;
  const qty=n($("entryQty").value);
  if(!food || qty<=0){ alert("Select a food and enter a quantity > 0."); return; }

  day.entries.push({
    id: (crypto.randomUUID ? crypto.randomUUID() : String(Date.now())+Math.random()),
    meal: $("mealType").value,
    category, food, unit, qty
  });
  setDayLog(dateKey, day);

  $("entryQty").value="";
  renderEntries();
  scheduleRefresh();
}

/* ===== Entries table ===== */
function renderEntries(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  const day=getDayLog(dateKey);

  const tb=$("entriesTbody");
  tb.innerHTML="";

  day.entries.forEach(e=>{
    const t=computeFood(e.food, n(e.qty), e.unit);
    const tr=document.createElement("tr");
    tr.innerHTML=`
      <td>${e.meal}</td>
      <td>${e.category || ""}</td>
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

  tb.querySelectorAll('input[data-field="qty"]').forEach(inp=>{
    inp.addEventListener("input", ()=>{
      const id=inp.dataset.id;
      const day2=getDayLog(dateKey);
      const row=day2.entries.find(x=>x.id===id);
      if(row){
        row.qty = n(inp.value);
        setDayLog(dateKey, day2);
        renderEntries();
        scheduleRefresh();
      }
    });
  });
  tb.querySelectorAll('button[data-action="del"]').forEach(btn=>{
    btn.addEventListener("click", ()=>{
      const id=btn.dataset.id;
      const day2=getDayLog(dateKey);
      day2.entries = day2.entries.filter(x=>x.id!==id);
      setDayLog(dateKey, day2);
      renderEntries();
      scheduleRefresh();
    });
  });

  const total=computeTotalsFromEntries();
  $("liveP").textContent = round1(total.P) + " g";
  $("liveC").textContent = round1(total.C) + " g";
  $("liveF").textContent = round1(total.F) + " g";
  $("liveK").textContent = Math.round(total.K) + " kcal";
}

/* ===== Save/Reset foods ===== */
function saveDay(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  alert("Saved for the Day ✔️ ("+dateKey+")");
}
function resetFoodLogs(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("logDate").value || getDefaultLogDate(profile);
  if(!confirm("Reset all food entries for "+dateKey+"?")) return;
  setDayLog(dateKey, { entries: [] });
  renderEntries();
  scheduleRefresh();
}

/* ===== Lifestyle ===== */
function loadAllLogs(){ 
  try{ return JSON.parse(localStorage.getItem(keyFor(LS_LOG)) || "{}"); }
  catch(e){ return {}; }
}
function saveAllLogs(obj){ localStorage.setItem(keyFor(LS_LOG), JSON.stringify(obj)); }

function getLifestyle(dateKey){
  const all=loadAllLifestyle();
  return all[dateKey] || {
    workoutType:"none", strengthSplit:"chest_triceps", workoutMins:0, burnKcal:0,
    sleepHours:0, sleepGoal:8, waterLiters:0, waterGoal:3
  };
}
function setLifestyle(dateKey, data){
  const all=loadAllLifestyle();
  all[dateKey]=data;
  saveAllLifestyle(all);
}

const BURN_PER_MIN = { none:0, walking:4.0, running:10.0, cycling:8.0, aerobic:7.0, cardio:7.5, hiit:12.0, strength:6.5 };
function calcBurn(){
  const type=$("workoutType").value;
  const mins=n($("workoutMins").value);
  const per=BURN_PER_MIN[type] ?? 0;
  return Math.round(per * mins);
}
function updateLifestyleUI(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("lifeDate").value || getDefaultLogDate(profile);
  const data=getLifestyle(dateKey);

  $("workoutType").value=data.workoutType || "none";
  $("strengthSplit").value=data.strengthSplit || "chest_triceps";
  $("workoutMins").value=data.workoutMins ?? 0;
  $("sleepHours").value=data.sleepHours ?? 0;
  $("sleepGoal").value(String(data.sleepGoal ?? 8));
  $("waterLiters").value=data.waterLiters ?? 0;
  $("waterGoal").value(String(data.waterGoal ?? 3));

  $("strengthSplitWrap").classList.toggle("hide", $("workoutType").value!=="strength");
  $("burnPreview").textContent = data.burnKcal ?? 0;

  const wLabel = $("workoutType").value==="strength"
    ? "Strength (" + $("strengthSplit").options[$("strengthSplit").selectedIndex].text + ")"
    : $("workoutType").options[$("workoutType").selectedIndex].text;

  $("lifeWorkoutLine").textContent = (wLabel==="None" ? "No workout" : wLabel) + " • " + (data.workoutMins||0) + " min • " + (data.burnKcal||0) + " kcal";
  $("lifeSleepWaterLine").textContent = (data.sleepHours||0) + "h sleep • " + (data.waterLiters||0) + "L water";
}
$("workoutType").addEventListener("change", ()=>{
  $("strengthSplitWrap").classList.toggle("hide", $("workoutType").value!=="strength");
  $("burnPreview").textContent = calcBurn();
});
["workoutMins","strengthSplit","sleepHours","sleepGoal","waterLiters","waterGoal"].forEach(id=>{
  $(id).addEventListener("input", ()=>{ $("burnPreview").textContent = calcBurn(); });
  $(id).addEventListener("change", ()=>{ $("burnPreview").textContent = calcBurn(); });
});
function saveLifestyle(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("lifeDate").value || getDefaultLogDate(profile);

  const data = {
    workoutType:$("workoutType").value,
    strengthSplit:$("strengthSplit").value,
    workoutMins:n($("workoutMins").value),
    burnKcal:calcBurn(),
    sleepHours:n($("sleepHours").value),
    sleepGoal:n($("sleepGoal").value),
    waterLiters:n($("waterLiters").value),
    waterGoal:n($("waterGoal").value),
  };

  setLifestyle(dateKey, data);
  $("lifeSavedNote").style.display="block";
  updateLifestyleUI();
  scheduleRefresh();
}
function resetLifestyle(){
  const profile=loadProfile() || getProfileDraft();
  const dateKey=$("lifeDate").value || getDefaultLogDate(profile);
  if(!confirm("Reset lifestyle inputs for "+dateKey+"?")) return;
  setLifestyle(dateKey, {
    workoutType:"none", strengthSplit:"chest_triceps", workoutMins:0, burnKcal:0,
    sleepHours:0, sleepGoal:8, waterLiters:0, waterGoal:3
  });
  $("lifeSavedNote").style.display="none";
  updateLifestyleUI();
  scheduleRefresh();
}
function resetToday(){
  const profile = loadProfile() || getProfileDraft();
  const dateKey = $("logDate").value || getDefaultLogDate(profile);
  const lifeKey = $("lifeDate").value || dateKey;

  if(!confirm(
    `Reset ONLY today (${dateKey}) for the active profile?\n\n`+
    `• Food entries for ${dateKey}\n`+
    `• Lifestyle for ${lifeKey}\n\n`+
    `Profile will NOT be deleted.`
  )) return;

  // Clear only this day
  setDayLog(dateKey, { entries: [] });

  setLifestyle(lifeKey, {
    workoutType:"none", strengthSplit:"chest_triceps", workoutMins:0, burnKcal:0,
    sleepHours:0, sleepGoal:8, waterLiters:0, waterGoal:3
  });

  // UI refresh
  renderEntries();
  updateLifestyleUI();
  refreshAll();

  alert("Today has been reset ✔️");
}

/* resetEverything(silent=false)
   - clears ALL data for the ACTIVE profile only */
function resetEverything(silent=false){
  if(!silent){
    if(!confirm(
      "This will reset ALL data for the ACTIVE profile on this device:\n\n" +
      "• Profile & targets\n" +
      "• Food logs (all days)\n" +
      "• Lifestyle data (all days)\n\n" +
      "This cannot be undone. Continue?"
    )) return;
  }

  // Clear active profile data only
  localStorage.removeItem(keyFor(LS_PROFILE));
  localStorage.removeItem(keyFor(LS_LOG));
  localStorage.removeItem(keyFor(LS_LIFE));

  // Clear dirty flags
  ["p_targetCalories","p_targetFats","p_targetCarbs"].forEach(id=>{
    if($(id)) $(id).dataset.dirty = "";
  });

  // Clear form fields
  $("p_name").value="";
  $("p_age").value="";
  $("p_sex").value="male";

  $("p_height_unit").value="ftin";
  $("p_weight_unit").value="kg";

  $("p_height_cm").value="";
  $("p_height_ft").value="";
  $("p_height_in").value="";
  $("p_weight_kg").value="";
  $("p_weight_lbs").value="";

  $("p_goal").value="loss";
  $("p_activity").value="1.2";
  $("p_dayStartHour").value="4";
  $("p_macroPreset").value="higherProtein";
  $("p_protMult").value="2.2";

  $("p_targetCalories").value="";
  $("p_targetProtein").value="";
  $("p_targetFats").value="";
  $("p_targetCarbs").value="";

  // Hide notes
  if($("profileSavedNote")) $("profileSavedNote").style.display="none";
  if($("profileSaveHint")) $("profileSaveHint").style.display="none";
  if($("lifeSavedNote")) $("lifeSavedNote").style.display="none";

  // Reset UI
  toggleHeightUI(false);
  toggleWeightUI(false);

  // Reset dates for this profile
  const p = getProfileDraft();
  const d = getDefaultLogDate(p);
  $("logDate").value = d;
  $("lifeDate").value = d;

  // Refresh
  renderEntries();
  updateLifestyleUI();
  refreshAll();

  if(!silent) alert("Active profile has been reset ✔️");
}

/* ===== BMI bar ===== */
function updateBMIBar(bmi){
  const fill=$("bmiFill");
  const pin=$("bmiPin");
  if(!bmi || bmi<=0){ fill.style.width="0%"; pin.style.left="0%"; pin.textContent="▲ You"; return; }
  const pct=clamp(((bmi-10)/(40-10))*100, 0, 100);
  fill.style.width=pct+"%";
  pin.style.left=pct+"%";
  pin.textContent="▲ You ("+round1(bmi)+")";
}

/* ===== Dashboard refresh ===== */
function refreshAll(){
  const profile=loadProfile() || getProfileDraft();

  if(!$("logDate").value) $("logDate").value = getDefaultLogDate(profile);
  if(!$("lifeDate").value) $("lifeDate").value = getDefaultLogDate(profile);

  const total=computeTotalsFromEntries();
  const life = getLifestyle($("lifeDate").value || getDefaultLogDate(profile));
  const burn = n(life.burnKcal);
  const netK = Math.max(0, total.K - burn);

  $("dashCalories").textContent = Math.round(total.K) + " kcal";
  $("dashBurn").textContent = Math.round(burn) + " kcal";
  $("dashNet").textContent = Math.round(netK);

  $("dashProtein").textContent = round1(total.P) + " g";
  $("dashCarbs").textContent   = round1(total.C) + " g";
  $("dashFat").textContent     = round1(total.F) + " g";

  const tCal=n(profile.targetCalories);
  const tP=n(profile.targetProtein);
  const tC=n(profile.targetCarbs);
  const tF=n(profile.targetFats);

  $("dashCalTarget").textContent  = tCal?Math.round(tCal):0;
  $("dashProtTarget").textContent = tP?Math.round(tP):0;
  $("dashCarbTarget").textContent = tC?Math.round(tC):0;
  $("dashFatTarget").textContent  = tF?Math.round(tF):0;

  if(!tCal){
    $("dashCalBalance").textContent="—";
    $("dashCalBalanceMsg").textContent="Complete Profile to calculate targets.";
    $("dashCalBalance").className="big";
  }else{
    const diff=Math.round(netK - tCal);
    if(diff>0){
      $("dashCalBalance").textContent="+"+diff+" kcal";
      $("dashCalBalance").className="big accentBad";
      $("dashCalBalanceMsg").textContent="Over target (net calories above target).";
    }else if(diff<0){
      $("dashCalBalance").textContent=diff+" kcal";
      $("dashCalBalance").className="big accentGood";
      $("dashCalBalanceMsg").textContent="Under target (net calories below target).";
    }else{
      $("dashCalBalance").textContent="0 kcal";
      $("dashCalBalance").className="big";
      $("dashCalBalanceMsg").textContent="Exactly on target.";
    }
  }

  function setMacroBalanceNumeric(diffG, valEl, msgEl, label){
    if(!isFinite(diffG)){
      valEl.textContent="—"; valEl.className="big";
      msgEl.textContent="—"; return;
    }
    if(diffG>0){
      valEl.textContent="+"+diffG+" g";
      valEl.className="big accentBad";
      msgEl.textContent="Over target " + label + ".";
    }else if(diffG<0){
      valEl.textContent=diffG+" g";
      valEl.className="big accentGood";
      msgEl.textContent="Under target " + label + ".";
    }else{
      valEl.textContent="0 g";
      valEl.className="big";
      msgEl.textContent="Exactly on target " + label + ".";
    }
  }

  setMacroBalanceNumeric(Math.round(total.P - tP), $("dashProtBalance"), $("dashProtBalanceMsg"), "protein");
  setMacroBalanceNumeric(Math.round(total.C - tC), $("dashCarbBalance"), $("dashCarbBalanceMsg"), "carbs");
  setMacroBalanceNumeric(Math.round(total.F - tF), $("dashFatBalance"), $("dashFatBalanceMsg"), "fat");

  $("dashBMI").textContent = profile.bmi ? round2(profile.bmi) : "—";
  $("dashBMR").textContent = profile.bmr ? Math.round(profile.bmr) : "—";
  $("dashTDEE").textContent = profile.tdee ? Math.round(profile.tdee) : "—";
  updateBMIBar(profile.bmi);

  $("dashWater").textContent = (n(life.waterLiters) || 0) + " L";
  $("dashWaterGoal").textContent = (n(life.waterGoal) || 0);
  $("dashSleep").textContent = (n(life.sleepHours) || 0) + " h";
  $("dashSleepGoal").textContent = (n(life.sleepGoal) || 0);
  $("dashLifeMsg").textContent = (burn>0 ? `Workout logged: ${burn} kcal burned` : "No workout logged");

  if(macroChart){
    macroChart.data.datasets[0].data = [round1(total.P), round1(total.C), round1(total.F)];
    macroChart.update();
  }
}

/* ===== PDF ===== */
async function savePDF(){
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ unit:"pt", format:"a4" });
  const profile = loadProfile() || getProfileDraft();
  const total = computeTotalsFromEntries();
  const life = getLifestyle($("lifeDate").value || getDefaultLogDate(profile));
  const burn = n(life.burnKcal);
  const netK = Math.max(0, total.K - burn);

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
  [
    `Name: ${profile.name || "—"}   |   Age: ${profile.age || "—"}   |   Sex: ${profile.sex || "—"}`,
    `BMI: ${profile.bmi ? round2(profile.bmi) : "—"} (Recommended 18.5–24.9)   |   BMR: ${profile.bmr ? Math.round(profile.bmr) : "—"} kcal   |   TDEE: ${profile.tdee ? Math.round(profile.tdee) : "—"} kcal`,
    `Goal: ${profile.goal || "—"}   |   Target Calories: ${profile.targetCalories ? Math.round(profile.targetCalories) : "—"} kcal`,
    `Targets — Protein: ${profile.targetProtein || "—"} g   |   Carbs: ${profile.targetCarbs || "—"} g   |   Fats: ${profile.targetFats || "—"} g`,
  ].forEach(t=>{ doc.text(t,40,y); y+=16; });

  y+=6;
  doc.setFont("helvetica","bold"); doc.text("Lifestyle", 40, y); y+=14;
  doc.setFont("helvetica","normal");
  doc.text(`Workout: ${life.workoutType || "none"} ${life.workoutType==="strength" ? "("+(life.strengthSplit||"")+")" : ""} • ${life.workoutMins||0} min • Burn: ${burn} kcal`, 40, y); y+=14;
  doc.text(`Sleep: ${life.sleepHours||0} h (Goal ${life.sleepGoal||0} h) • Water: ${life.waterLiters||0} L (Goal ${life.waterGoal||0} L)`, 40, y); y+=18;

  doc.setFont("helvetica","bold"); doc.text("Today’s Totals", 40, y); y+=14;
  doc.setFont("helvetica","normal");
  doc.text(`Calories eaten: ${Math.round(total.K)} kcal   |   Burned: ${Math.round(burn)} kcal   |   Net: ${Math.round(netK)} kcal`, 40, y); y+=14;
  doc.text(`Protein: ${round1(total.P)} g   |   Carbs: ${round1(total.C)} g   |   Fat: ${round1(total.F)} g`, 40, y);
  y+=18;

  doc.setFont("helvetica","bold"); doc.text("Macro Chart", 40, y); y+=10;
  const imgData = $("macroChart").toDataURL("image/png", 1.0);
  doc.addImage(imgData, "PNG", 40, y, pageW-80, 220);

  doc.save("Nowshad_Macro_Report.pdf");
}

/* ===== Init ===== */
(function init(){
  ensureProfileSystem();
rebuildProfileSelect();

  const p = loadProfile() || getProfileDraft();

  toggleHeightUI(false);
  toggleWeightUI(false);
  updateProfilePreviewOnly();

  $("logDate").value = getDefaultLogDate(p);
  $("logDate").addEventListener("change", ()=>{ renderEntries(); scheduleRefresh(); });

  buildCategorySelect();

  // Do NOT force "Chicken" if it's not present for some reason; pick first category safely
  const firstCat = Object.keys(CATEGORY_ITEMS)[0] || "Chicken";
  $("entryCategory").value = CATEGORY_ITEMS["Chicken"] ? "Chicken" : firstCat;

  buildFoodSelectForCategory($("entryCategory").value);
  buildUnitSelect($("entryFood").value);
  updateEntryPreview();

  $("lifeDate").value = getDefaultLogDate(p);
  $("lifeDate").addEventListener("change", ()=>{ updateLifestyleUI(); scheduleRefresh(); });
  updateLifestyleUI();

  renderEntries();
  refreshAll();
})();
  function resetEverything(){
  if(!confirm(
    "This will completely reset ALL data on this device:\n\n" +
    "• Profile & targets\n" +
    "• Food logs (all days)\n" +
    "• Lifestyle data (all days)\n\n" +
    "This cannot be undone. Continue?"
  )) return;

  // Clear all stored data
  localStorage.removeItem(LS_PROFILE);
  localStorage.removeItem(LS_LOG);
  localStorage.removeItem(LS_LIFE);

  // Clear dirty flags (so auto-calculation works again)
  ["p_targetCalories","p_targetFats","p_targetCarbs"].forEach(id=>{
    if($(id)) $(id).dataset.dirty = "";
  });

  // Reset profile UI fields
  $("p_name").value="";
  $("p_age").value="";
  $("p_sex").value="male";

  $("p_height_unit").value="ftin";
  $("p_weight_unit").value="kg";

  $("p_height_cm").value="";
  $("p_height_ft").value="";
  $("p_height_in").value="";
  $("p_weight_kg").value="";
  $("p_weight_lbs").value="";

  $("p_goal").value="loss";
  $("p_activity").value="1.2";
  $("p_dayStartHour").value="4";
  $("p_macroPreset").value="higherProtein";
  $("p_protMult").value="2.2";

  $("p_targetCalories").value="";
  $("p_targetProtein").value="";
  $("p_targetFats").value="";
  $("p_targetCarbs").value="";

  // Reset food inputs
  if($("entryQty")) $("entryQty").value="";
  if($("mealType")) $("mealType").value="Lunch";

  // Reset lifestyle inputs
  if($("workoutType")) $("workoutType").value="none";
  if($("workoutMins")) $("workoutMins").value=0;
  if($("sleepHours")) $("sleepHours").value="";
  if($("waterLiters")) $("waterLiters").value="";

  // Hide saved messages
  if($("profileSavedNote")) $("profileSavedNote").style.display="none";
  if($("profileSaveHint")) $("profileSaveHint").style.display="none";
  if($("lifeSavedNote")) $("lifeSavedNote").style.display="none";

  // Reset UI state
  toggleHeightUI(false);
  toggleWeightUI(false);

  // Reset dates
  const today = new Date().toISOString().slice(0,10);
  $("logDate").value = today;
  $("lifeDate").value = today;

  // Re-render everything cleanly
  renderEntries();
  refreshAll();

  alert("All data has been reset ✔️\nYou can now enter a new user from Profile.");
}

</script>

</body>
</html>
