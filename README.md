
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Nowshad's Macro Calculator</title>

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">

  <!-- Libraries -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <style>
    :root{
      --bg1:#0b3aa8; --bg2:#071a4b;
      --card:rgba(255,255,255,.10);
      --card2:rgba(255,255,255,.14);
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
    a{color:#cfe2ff}
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
    input::placeholder, textarea::placeholder{color:rgba(238,243,255,.55)}
    .btn{
      padding:11px 12px; border-radius:12px; border:1px solid var(--stroke);
      background:rgba(255,255,255,.14); color:var(--text);
      cursor:pointer; font-weight:700;
    }
    .btn:hover{background:rgba(255,255,255,.18)}
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

    /* BMI bar */
    .bmiBar{
      height:10px; border-radius:999px; overflow:hidden; background:rgba(255,255,255,.14);
      border:1px solid rgba(255,255,255,.16);
    }
    .bmiFill{height:100%; width:0%; background:linear-gradient(90deg, var(--bad), var(--warn), var(--good), var(--warn), var(--bad))}
    .bmiMarker{
      position:relative; height:14px; margin-top:6px;
    }
    .bmiMarker span{
      position:absolute; top:0; transform:translateX(-50%);
      font-size:11px; color:var(--muted)
    }

    /* List rows for multi-items */
    .multiList{display:flex; flex-direction:column; gap:8px}
    .multiItem{
      display:grid; grid-template-columns:22px 1fr 110px;
      gap:10px; align-items:center;
      padding:10px; border:1px solid rgba(255,255,255,.16);
      background:rgba(255,255,255,.06); border-radius:14px;
    }
    .multiItem small{color:var(--muted); display:block; font-size:11px; margin-top:2px}
    .chk{width:18px; height:18px}
    .qtyWrap{display:flex; gap:8px; align-items:center}
    .qtyWrap input{padding:10px 10px; border-radius:12px}
    .unitTag{font-size:12px; color:var(--muted); min-width:38px; text-align:right}

    /* Chart */
    .chartBox{
      height:260px;
      border:1px solid rgba(255,255,255,.16);
      border-radius:16px;
      background:rgba(255,255,255,.06);
      padding:10px;
    }
    canvas{max-width:100% !important}

    /* Footer note */
    .note{
      margin-top:10px;
      padding:10px 12px;
      border-radius:14px;
      background:rgba(61,220,151,.12);
      border:1px solid rgba(61,220,151,.28);
      color:#dffaf0;
      font-size:12px;
    }
    .warnNote{
      background:rgba(255,204,102,.12);
      border:1px solid rgba(255,204,102,.26);
      color:#fff3da;
    }
  </style>
</head>

<body>
  <div class="wrap">
    <div class="topbar">
      <div class="brand">
        <h1>Nowshad's Macro Calculator</h1>
        <span>Profile → Food Log → Dashboard summary + PDF report</span>
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
        <div class="sub">Tip: fill Profile once → then log foods. Everything auto-calculates. Use “Save PDF” anytime.</div>

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
        <div class="divider"></div>

        <h3>Sleep • Water • Exercise</h3>
        <div class="row cols3">
          <div>
            <label>Sleep (hours)</label>
            <input type="number" min="0" step="0.1" id="sleepHours" placeholder="e.g., 7.5">
          </div>
          <div>
            <label>Water (liters)</label>
            <input type="number" min="0" step="0.1" id="waterLiters" placeholder="e.g., 3.0">
          </div>
          <div>
            <label>Exercise calories burned</label>
            <input type="number" min="0" step="1" id="exerciseCals" placeholder="e.g., 350">
          </div>
        </div>

        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Workout type</label>
            <select id="workoutType">
              <option value="">— Select —</option>
              <option value="Cardio">Cardio</option>
              <option value="HIIT">HIIT</option>
              <option value="Strength">Strength training</option>
              <option value="Sports">Sports</option>
              <option value="Other">Other</option>
            </select>
          </div>
          <div>
            <label>Workout time (minutes)</label>
            <input type="number" min="0" step="1" id="workoutMinutes" placeholder="e.g., 45">
          </div>
        </div>

        <div id="strengthSplitWrap" class="row cols2 hide" style="margin-top:10px">
          <div>
            <label>Strength split</label>
            <select id="strengthSplit">
              <option value="">— Select —</option>
              <option>Chest + Triceps</option>
              <option>Back + Biceps</option>
              <option>Shoulders + Forearm</option>
              <option>Legs</option>
              <option>Full Body</option>
            </select>
          </div>
          <div>
            <label>Notes</label>
            <input id="workoutNotes" placeholder="e.g., PRs, intensity, etc.">
          </div>
        </div>

        <div class="warnNote" style="margin-top:10px">
          Recommendations (simple):
          <ul style="margin:8px 0 0 18px; padding:0; color:inherit; font-size:12px;">
            <li>Sleep goal: 7–9 hours (most adults).</li>
            <li>Water: aim ~2.5–3.5 L/day (adjust for heat/training).</li>
            <li>For weight gain: stay consistent with a calorie surplus and protein target.</li>
          </ul>
        </div>
      </div>
    </section>

    <!-- PROFILE -->
    <section id="page-profile" class="grid cols2 hide" style="margin-top:14px;">
      <div class="card">
        <h2>Profile</h2>
        <div class="sub">Fill once → Save → the dashboard + food targets update automatically.</div>

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
              <option value="cm">Centimeters (cm)</option>
              <option value="ftin">Feet/Inches</option>
            </select>
          </div>
          <div>
            <label>Weight unit</label>
            <select id="p_weight_unit">
              <option value="kg">Kilograms (kg)</option>
              <option value="lbs">Pounds (lbs)</option>
            </select>
          </div>
        </div>

        <div class="row cols3" style="margin-top:10px" id="height_cm_row">
          <div>
            <label>Height (cm)</label>
            <input type="number" min="0" step="0.1" id="p_height_cm" placeholder="e.g., 175">
          </div>
          <div class="hide"></div>
          <div class="hide"></div>
        </div>

        <div class="row cols3 hide" style="margin-top:10px" id="height_ftin_row">
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
            <label>Activity level (workout days guidance)</label>
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
            <label>Day starts at (hour) — fixes late-night logging</label>
            <input type="number" min="0" max="23" step="1" id="p_dayStartHour" placeholder="e.g., 4" value="4">
          </div>
          <div>
            <label>Sex (for BMR formula)</label>
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
        <div class="sub">
          Protein reference is set slightly above the minimum recommended range (so if range is 161–214, default becomes 200).
        </div>

        <div class="row cols3" style="margin-top:10px">
          <div>
            <label>Target calories (auto)</label>
            <input type="number" min="0" step="1" id="p_targetCalories" placeholder="Auto">
          </div>
          <div>
            <label>Target protein (g) — editable</label>
            <input type="number" min="0" step="1" id="p_targetProtein" placeholder="Auto">
          </div>
          <div>
            <label>Target carbs (g) — editable</label>
            <input type="number" min="0" step="1" id="p_targetCarbs" placeholder="Auto">
          </div>
        </div>

        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Target fats (g) — editable</label>
            <input type="number" min="0" step="1" id="p_targetFats" placeholder="Auto">
          </div>
          <div>
            <label>Protein multiplier (g/kg) for reference</label>
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
        <button class="btn" style="margin-left:8px" onclick="resetAll()">Reset (profile + today log)</button>

        <div class="note" id="profileSaveHint" style="display:none;">
          Saved ✔️ Go to Food Log and start logging. Dashboard updates automatically.
        </div>

        <div class="divider"></div>
        <div class="sub">
          Data sources are shown in-app in the footer (butter, oils, etc.). Activity guidance text aligns with Calculator.net’s TDEE page. 
        </div>
      </div>

      <div class="card">
        <h2>Metrics (BMI • BMR • TDEE)</h2>
        <div class="sub">Single-line display. Updates as profile fields change.</div>

        <div class="divider"></div>

        <div class="row cols3">
          <div class="kpi">
            <div class="cap">BMI</div>
            <div class="big" id="p_bmiLine">—</div>
            <div class="cap">Recommended: 18.5–24.9</div>
          </div>
          <div class="kpi">
            <div class="cap">BMR</div>
            <div class="big" id="p_bmrLine">—</div>
            <div class="cap">Mifflin–St Jeor</div>
          </div>
          <div class="kpi">
            <div class="cap">TDEE</div>
            <div class="big" id="p_tdeeLine">—</div>
            <div class="cap" id="p_targetLine">Target: —</div>
          </div>
        </div>

        <div class="divider"></div>

        <div class="warnNote">
          Activity definitions + exercise duration guidance follow Calculator.net’s TDEE page (e.g., Light 1–3x/week, Moderate 4–5x/week; exercise ~15–30 min elevated heart rate; intense 45–120 min). 
        </div>
      </div>
    </section>

    <!-- FOOD LOG -->
    <section id="page-food" class="grid cols2 hide" style="margin-top:14px;">
      <div class="card">
        <h2>Food Log</h2>
        <div class="sub">
          Choose your log day if needed (useful for late-night entries). Items auto-calc and roll into Dashboard.
        </div>

        <div class="divider"></div>

        <div class="row cols2">
          <div>
            <label>Log date</label>
            <input type="date" id="logDate">
          </div>
          <div>
            <label>Meal type (applies to section entries)</label>
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

        <!-- PROTEIN -->
        <h3>Protein — Chicken (separate section)</h3>
        <div class="sub">Instant section preview updates as you type.</div>
        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Chicken Breast (grams)</label>
            <input type="number" min="0" step="1" id="chicken_breast" placeholder="Enter grams">
          </div>
          <div>
            <label>Chicken Thigh (grams)</label>
            <input type="number" min="0" step="1" id="chicken_thigh" placeholder="Enter grams">
          </div>
          <div>
            <label>Chicken Wings (grams)</label>
            <input type="number" min="0" step="1" id="chicken_wings" placeholder="Enter grams">
          </div>
          <div>
            <label>Chicken Drumsticks (grams)</label>
            <input type="number" min="0" step="1" id="chicken_drumsticks" placeholder="Enter grams">
          </div>
        </div>
        <div class="pill" style="margin-top:10px">
          Chicken subtotal: <b><span id="prevChickenP">0</span>g P</b> • <b><span id="prevChickenF">0</span>g F</b> • <b><span id="prevChickenC">0</span>g C</b> • <b><span id="prevChickenK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <h3>Protein — Other protein sources</h3>
        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Eggs (count)</label>
            <input type="number" min="0" step="1" id="egg" placeholder="Enter count">
          </div>
          <div>
            <label>Protein Shake (scoops)</label>
            <input type="number" min="0" step="1" id="protein_shake" placeholder="Enter scoops">
          </div>
          <div>
            <label>Mutton (grams)</label>
            <input type="number" min="0" step="1" id="mutton" placeholder="Enter grams">
          </div>
          <div>
            <label>Duck (grams)</label>
            <input type="number" min="0" step="1" id="duck" placeholder="Enter grams">
          </div>
          <div>
            <label>Pigeon (grams)</label>
            <input type="number" min="0" step="1" id="pigeon" placeholder="Enter grams">
          </div>
          <div>
            <label>Quail (grams)</label>
            <input type="number" min="0" step="1" id="quail" placeholder="Enter grams">
          </div>
          <div>
            <label>Beef (grams)</label>
            <input type="number" min="0" step="1" id="beef" placeholder="Enter grams">
          </div>
          <div>
            <label>Fish (grams)</label>
            <input type="number" min="0" step="1" id="fish" placeholder="Enter grams">
          </div>
          <div>
            <label>Shrimp (grams)</label>
            <input type="number" min="0" step="1" id="shrimp" placeholder="Enter grams">
          </div>
          <div>
            <label>Dried Fish / Bombay Duck (grams)</label>
            <input type="number" min="0" step="1" id="dried_fish" placeholder="Enter grams">
          </div>
        </div>
        <div class="pill" style="margin-top:10px">
          Other protein subtotal: <b><span id="prevProteinP">0</span>g P</b> • <b><span id="prevProteinF">0</span>g F</b> • <b><span id="prevProteinC">0</span>g C</b> • <b><span id="prevProteinK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <!-- CARBS -->
        <h3>Carbs</h3>
        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Rice (grams)</label>
            <input type="number" min="0" step="1" id="rice_g" placeholder="Enter grams cooked rice">
          </div>
          <div>
            <label>Rice (plate portion)</label>
            <select id="rice_plate">
              <option value="0">0</option>
              <option value="0.25">1/4 plate</option>
              <option value="0.5">1/2 plate</option>
              <option value="1">1 plate</option>
              <option value="1.5">1.5 plates</option>
              <option value="2">2 plates</option>
            </select>
          </div>
          <div>
            <label>Bread (slices)</label>
            <input type="number" min="0" step="1" id="bread" placeholder="Enter slices">
          </div>
          <div>
            <label>Roti (pcs)</label>
            <input type="number" min="0" step="1" id="roti" placeholder="Enter pcs">
          </div>
          <div>
            <label>Paratha (pcs)</label>
            <input type="number" min="0" step="1" id="paratha" placeholder="Enter pcs">
          </div>
          <div>
            <label>Oats (grams dry)</label>
            <input type="number" min="0" step="1" id="oats_g" placeholder="Enter grams">
          </div>
          <div>
            <label>Milk Tea (cups)</label>
            <input type="number" min="0" step="1" id="milk_tea" placeholder="Enter cups">
          </div>
          <div>
            <label>Carbonated Beverage (ml)</label>
            <input type="number" min="0" step="10" id="carbonated_beverage_ml" placeholder="e.g., 330">
          </div>
        </div>
        <div class="pill" style="margin-top:10px">
          Carbs subtotal: <b><span id="prevCarbP">0</span>g P</b> • <b><span id="prevCarbF">0</span>g F</b> • <b><span id="prevCarbC">0</span>g C</b> • <b><span id="prevCarbK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <!-- VEGETABLES -->
        <h3>Vegetables & Salad</h3>
        <div class="row cols2" style="margin-top:10px">
          <div>
            <label>Salad (cups)</label>
            <input type="number" min="0" step="0.5" id="salad" placeholder="Enter cups">
          </div>
          <div>
            <label>Vegetables (cups)</label>
            <input type="number" min="0" step="0.5" id="vegetables" placeholder="Enter cups">
          </div>
        </div>
        <div class="pill" style="margin-top:10px">
          Veg subtotal: <b><span id="prevVegP">0</span>g P</b> • <b><span id="prevVegF">0</span>g F</b> • <b><span id="prevVegC">0</span>g C</b> • <b><span id="prevVegK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <!-- FATS -->
        <h3>Fats (you can select multiple)</h3>
        <div class="sub">Olive oil + soybean oil + butter + ghee can all be logged together.</div>

        <div class="multiList" id="oilList" style="margin-top:10px"></div>

        <div class="pill" style="margin-top:10px">
          Fats subtotal: <b><span id="prevFatP">0</span>g P</b> • <b><span id="prevFatF">0</span>g F</b> • <b><span id="prevFatC">0</span>g C</b> • <b><span id="prevFatK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <!-- Snacks & Fruits -->
        <h3>Snacks & Fruits</h3>

        <div class="row cols2" style="margin-top:10px">
          <div class="card" style="padding:12px; background:rgba(255,255,255,.06); box-shadow:none">
            <h3 style="margin:0 0 8px">Dates (Ajwa small / Deglet Noor style)</h3>
            <div class="sub">
              Pick grams OR pcs. “Small Ajwa” reference uses ~9g/date (vendor spec); you can edit grams-per-piece. 
            </div>
            <div class="row cols2" style="margin-top:10px">
              <div>
                <label>Dates (grams)</label>
                <input type="number" min="0" step="1" id="dates_g" placeholder="Enter grams">
              </div>
              <div>
                <label>Dates (pcs)</label>
                <input type="number" min="0" step="1" id="dates_pcs" placeholder="Enter pcs">
              </div>
              <div>
                <label>Type</label>
                <select id="dates_type">
                  <option value="deglet" selected>Deglet Noor (default)</option>
                  <option value="ajwaSmall">Ajwa (small)</option>
                  <option value="medjool">Medjool (large)</option>
                </select>
              </div>
              <div>
                <label>Grams per date (editable)</label>
                <input type="number" min="1" step="1" id="dates_gPer" value="8">
              </div>
            </div>
          </div>

          <div class="card" style="padding:12px; background:rgba(255,255,255,.06); box-shadow:none">
            <h3 style="margin:0 0 8px">Peanut Butter + Yogurt/Curd</h3>
            <div class="row cols2" style="margin-top:10px">
              <div>
                <label>Peanut Butter (tbsp)</label>
                <input type="number" min="0" step="0.5" id="peanut_butter_tbsp" placeholder="e.g., 2">
              </div>
              <div>
                <label>Yogurt / Curd (grams)</label>
                <input type="number" min="0" step="10" id="yogurt_g" placeholder="e.g., 200">
              </div>
              <div>
                <label>Chocolate (servings)</label>
                <input type="number" min="0" step="0.5" id="chocolate_serv" placeholder="e.g., 1">
              </div>
              <div>
                <label>Cake (servings)</label>
                <input type="number" min="0" step="0.5" id="cake_serv" placeholder="e.g., 1">
              </div>
            </div>
          </div>
        </div>

        <div class="divider"></div>

        <h3>Nuts (select multiple)</h3>
        <div class="multiList" id="nutsList" style="margin-top:10px"></div>

        <div class="divider"></div>

        <h3>Fruits (select multiple)</h3>
        <div class="multiList" id="fruitsList" style="margin-top:10px"></div>

        <div class="pill" style="margin-top:10px">
          Snacks & Fruits subtotal: <b><span id="prevSnackP">0</span>g P</b> • <b><span id="prevSnackF">0</span>g F</b> • <b><span id="prevSnackC">0</span>g C</b> • <b><span id="prevSnackK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <!-- Meals -->
        <h3>Meals (plate portions)</h3>
        <div class="sub">Choose 1/4, 1/2, 1 plate or more.</div>
        <div class="multiList" id="mealsList" style="margin-top:10px"></div>

        <div class="pill" style="margin-top:10px">
          Meals subtotal: <b><span id="prevMealsP">0</span>g P</b> • <b><span id="prevMealsF">0</span>g F</b> • <b><span id="prevMealsC">0</span>g C</b> • <b><span id="prevMealsK">0</span> kcal</b>
        </div>

        <div class="divider"></div>

        <button class="btn" onclick="saveTodayLog()">Save Today Log</button>
        <button class="btn" style="margin-left:8px" onclick="clearTodayLog()">Clear Today Log</button>

        <div class="divider"></div>
        <div class="sub">
          Disclaimer: values are estimates. For medical decisions, consult a qualified professional.
        </div>
      </div>

      <!-- Right column: live totals + instructions + sources -->
      <div class="card">
        <h2>Live Totals (Auto)</h2>
        <div class="sub">This updates instantly from all sections + stores today’s log by date.</div>

        <div class="divider"></div>

        <div class="row cols2">
          <div class="kpi">
            <div class="cap">Protein</div>
            <div class="big" id="liveP">0 g</div>
          </div>
          <div class="kpi">
            <div class="cap">Carbs</div>
            <div class="big" id="liveC">0 g</div>
          </div>
          <div class="kpi">
            <div class="cap">Fat</div>
            <div class="big" id="liveF">0 g</div>
          </div>
          <div class="kpi">
            <div class="cap">Calories</div>
            <div class="big" id="liveK">0 kcal</div>
          </div>
        </div>

        <div class="divider"></div>

        <h3>How to use</h3>
        <ol style="margin:8px 0 0 18px; color:var(--muted); font-size:12px; line-height:1.4">
          <li>Go to <b>Profile</b>, enter your details, pick goal + activity, then Save.</li>
          <li>Go to <b>Food Log</b>, enter foods. Totals update instantly.</li>
          <li>Check <b>Dashboard</b> for balances (over/under target) + chart.</li>
          <li>Use <b>Save PDF</b> to generate a shareable report.</li>
        </ol>

        <div class="divider"></div>

        <h3>Data sources (key items)</h3>
        <div class="sub">
          Butter per 1 tbsp (14g) 102 kcal. Olive oil 1 tbsp 119 kcal. Soybean oil 1 tbsp 120 kcal. Peanut butter (USDA label) 2 tbsp 200 kcal.
          Activity guidance lines are based on Calculator.net’s TDEE page.
        </div>

        <div class="divider"></div>

        <div class="sub">
          If you want “no arguments with users” accuracy: replace approximations with USDA FoodData Central item IDs and lock serving sizes.
        </div>
      </div>
    </section>

    <div class="card" style="margin-top:14px">
      <div class="sub">
        Sources used for “reliable” macros where possible:
        USDA/FNS butter sheet, MyFoodData (USDA-derived) olive oil & ghee, USDA/label peanut butter, Calculator.net activity definitions.
      </div>
    </div>
  </div>

<script>
/* =========================
   Storage keys
========================= */
const LS_PROFILE = "nowshad_macro_profile_v3";
const LS_LOG = "nowshad_macro_dailylog_v3";

/* =========================
   Helpers
========================= */
const $ = (id)=>document.getElementById(id);
const clamp = (v,min,max)=>Math.max(min,Math.min(max,v));
const n = (v)=>isFinite(parseFloat(v)) ? parseFloat(v) : 0;
function round1(x){ return Math.round(x*10)/10; }
function round2(x){ return Math.round(x*100)/100; }

/* =========================
   Tabs / Routing
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
   Food database
   NOTE: Keep simple approximations for your original items,
   and use "reliable" sources for oils/butter/peanut butter etc.
========================= */
const FOOD = {
  // ---- Protein shake / eggs (kept from your original style) ----
  egg: { unit:"count", perUnit:{P:6, F:5, C:0.6, K:72} },              // approximation
  protein_shake: { unit:"scoop", perUnit:{P:25, F:2, C:3, K:120} },     // approximation

  // ---- Chicken (per 100g) (kept from your original style) ----
  chicken_breast: { unit:"g", per100:{P:31, F:3.6, C:0, K:165} },
  chicken_thigh: { unit:"g", per100:{P:26, F:8, C:0, K:209} },
  chicken_wings: { unit:"g", per100:{P:30, F:13, C:0, K:290} },
  chicken_drumsticks: { unit:"g", per100:{P:28, F:10, C:0, K:215} },

  // ---- Other protein sources (per 100g) approximations ----
  mutton: { unit:"g", per100:{P:25, F:20, C:0, K:294} },
  duck: { unit:"g", per100:{P:23, F:28, C:0, K:337} },
  pigeon: { unit:"g", per100:{P:25, F:15, C:0, K:250} },
  quail: { unit:"g", per100:{P:22, F:12, C:0, K:210} },
  beef: { unit:"g", per100:{P:26, F:15, C:0, K:250} },
  fish: { unit:"g", per100:{P:22, F:5, C:0, K:120} },
  shrimp: { unit:"g", per100:{P:24, F:2, C:0, K:99} },
  dried_fish: { unit:"g", per100:{P:60, F:5, C:0, K:300} },

  // ---- Carbs / staples ----
  rice_g: { unit:"g", per100:{P:2.7, F:0.3, C:28, K:130} }, // cooked white rice approx
  rice_plate: { unit:"plate", perPlate:{P:8, F:1, C:90, K:420} }, // 1 plate cooked rice approx
  bread: { unit:"slice", perUnit:{P:3, F:1, C:15, K:80} }, // original style

  roti: { unit:"pcs", perUnit:{P:3.5, F:3, C:18, K:120} }, // approximation
  paratha: { unit:"pcs", perUnit:{P:6, F:12, C:30, K:260} }, // approximation
  oats_g: { unit:"g", per100:{P:16.9, F:6.9, C:66.3, K:389} }, // 100g oats (commonly cited; USDA-like) 

  milk_tea: { unit:"cup", perUnit:{P:2, F:3, C:10, K:80} }, // approximation
  carbonated_beverage_ml: { unit:"ml", per100:{P:0, F:0, C:10.6, K:42} }, // ~ cola per 100ml approx

  // ---- Vegetables ----
  salad: { unit:"cup", perUnit:{P:0.5, F:0.1, C:2, K:15} }, // original style
  vegetables: { unit:"cup", perUnit:{P:2, F:0.2, C:5, K:35} }, // original style

  // ---- Snacks extras ----
  yogurt_g: { unit:"g", per100:{P:3.5, F:3.3, C:4.7, K:61} }, // plain yogurt approx
  chocolate_serv: { unit:"serv", perUnit:{P:2, F:12, C:25, K:220} }, // generic serving approx
  cake_serv: { unit:"serv", perUnit:{P:3, F:10, C:30, K:220} }, // generic serving approx
};

/* =========================
   Multi-select lists (oils, nuts, fruits, meals)
========================= */

// Oils: reliable numbers where possible
// Butter: 1 tbsp (14g) 102 kcal, fat 11.5g, protein 0.12g, carbs ~0  (USDA/FNS / MyFoodData style)
// Olive oil: 1 tbsp (14g) 119 kcal, fat 13.5g
// Soybean oil: 1 tbsp (14g) 120 kcal, fat 13.6g
// Ghee: 1 tbsp (14g) 126 kcal, fat 14g
const OILS = [
  { key:"butter_tbsp", name:"Butter", unit:"tbsp", perUnit:{P:0.12, F:11.5, C:0.01, K:102} },
  { key:"olive_oil_tbsp", name:"Olive oil", unit:"tbsp", perUnit:{P:0, F:13.5, C:0, K:119} },
  { key:"soy_oil_tbsp", name:"Soybean oil", unit:"tbsp", perUnit:{P:0, F:13.6, C:0, K:120} },
  { key:"ghee_tbsp", name:"Ghee", unit:"tbsp", perUnit:{P:0, F:14, C:0, K:126} },
];

// Nuts (per 28g / 1 oz approximations)
const NUTS = [
  { key:"almonds", name:"Almonds", unit:"g", per100:{P:21.2, F:49.9, C:21.6, K:579} },
  { key:"peanuts", name:"Peanuts", unit:"g", per100:{P:25.8, F:49.2, C:16.1, K:567} },
  { key:"cashews", name:"Cashews", unit:"g", per100:{P:18.2, F:43.9, C:30.2, K:553} },
  { key:"walnuts", name:"Walnuts", unit:"g", per100:{P:15.2, F:65.2, C:13.7, K:654} },
  { key:"pistachios", name:"Pistachios", unit:"g", per100:{P:20.2, F:45.3, C:27.2, K:562} },
];

// Fruits: per 1 medium item (approx) or per100
const FRUITS = [
  { key:"banana", name:"Banana (1 medium ~118g)", unit:"pcs", perUnit:{P:1.3, F:0.4, C:27, K:105} },
  { key:"apple", name:"Apple (1 medium ~182g)", unit:"pcs", perUnit:{P:0.5, F:0.3, C:25, K:95} },
  { key:"orange", name:"Orange (1 medium ~131g)", unit:"pcs", perUnit:{P:1.2, F:0.2, C:15.4, K:62} },
  { key:"mango", name:"Mango (1 cup slices ~165g)", unit:"cups", perUnit:{P:1.4, F:0.6, C:25, K:99} },
  { key:"grapes", name:"Grapes (1 cup ~151g)", unit:"cups", perUnit:{P:1.1, F:0.2, C:27.3, K:104} },
];

// Peanut butter is in snacks (tbsp). Use USDA label: 2 tbsp 200 kcal; so 1 tbsp ~100 kcal, P 3.5, F 8, C 4
// We'll implement per tbsp
const PB_PER_TBSP = {P:3.5, F:8, C:4, K:100};

// Meals (plate-based). For fast-food branded items, numbers vary by region;
// keep “standard” as best-effort and label as estimates.
const MEALS = [
  { key:"kacchi_biryani", name:"Kacchi Biryani", perPlate:{P:26, F:39, C:90, K:800} },
  { key:"biryani", name:"Biryani (general)", perPlate:{P:20, F:15, C:85, K:600} },
  { key:"khichuri", name:"Khichuri", perPlate:{P:14, F:12, C:70, K:450} },
  { key:"fried_rice", name:"Fried rice", perPlate:{P:12, F:14, C:75, K:520} },
  { key:"shawarma", name:"Shawarma (wrap)", perPlate:{P:30, F:20, C:40, K:450} },
  { key:"chicken_broast", name:"Chicken broast", perPlate:{P:35, F:25, C:25, K:550} },
  { key:"beef_steak", name:"Beef steak meal", perPlate:{P:45, F:25, C:10, K:500} },
  { key:"fried_chicken_meal", name:"Fried chicken meal (KFC style)", perPlate:{P:35, F:30, C:45, K:700} },
  { key:"whopper", name:"Burger King Whopper", perPlate:{P:30.4, F:38.4, C:51.1, K:678.8} },
  { key:"double_whopper", name:"Burger King Double Whopper", perPlate:{P:46, F:52, C:46, K:838} },
];

// Portion options for meals
const PLATE_OPTIONS = [
  { label:"0", v:0 },
  { label:"1/4", v:0.25 },
  { label:"1/2", v:0.5 },
  { label:"1", v:1 },
  { label:"1.5", v:1.5 },
  { label:"2", v:2 },
];

/* =========================
   UI builders
========================= */
function buildMultiList(containerId, items, qtyDefault=0){
  const el = $(containerId);
  el.innerHTML = "";
  items.forEach(it=>{
    const row = document.createElement("div");
    row.className = "multiItem";
    row.innerHTML = `
      <input class="chk" type="checkbox" id="${it.key}_chk">
      <div>
        <div style="font-weight:700">${it.name}</div>
        <small>${it.unit === "g" ? "Enter grams" : it.unit === "tbsp" ? "Enter tbsp" : it.unit}</small>
      </div>
      <div class="qtyWrap">
        <input type="number" min="0" step="${it.unit==='g'||it.unit==='ml' ? 1 : 0.5}" id="${it.key}_qty" value="${qtyDefault}" />
        <div class="unitTag">${it.unit}</div>
      </div>
    `;
    el.appendChild(row);
  });
}

function buildMealsList(){
  const el = $("mealsList");
  el.innerHTML = "";
  MEALS.forEach(m=>{
    const row = document.createElement("div");
    row.className = "multiItem";
    row.style.gridTemplateColumns = "22px 1fr 120px";
    const opts = PLATE_OPTIONS.map(o=>`<option value="${o.v}">${o.label}</option>`).join("");
    row.innerHTML = `
      <input class="chk" type="checkbox" id="${m.key}_chk">
      <div>
        <div style="font-weight:700">${m.name}</div>
        <small>Plate-based logging</small>
      </div>
      <div>
        <select id="${m.key}_plate">${opts}</select>
      </div>
    `;
    el.appendChild(row);
  });
}

buildMultiList("oilList", OILS, 0);
buildMultiList("nutsList", NUTS, 0);
buildMultiList("fruitsList", FRUITS, 0);
buildMealsList();

/* =========================
   Profile calculations
========================= */
function getProfileDraft(){
  const sex = $("p_sex").value;
  const age = n($("p_age").value);
  const weightUnit = $("p_weight_unit").value;
  const heightUnit = $("p_height_unit").value;

  let wkg = weightUnit==="kg" ? n($("p_weight_kg").value) : n($("p_weight_lbs").value) * 0.45359237;
  let hm = 0;
  if(heightUnit==="cm"){
    hm = n($("p_height_cm").value)/100;
  }else{
    hm = (n($("p_height_ft").value)*0.3048) + (n($("p_height_in").value)*0.0254);
  }
  const hcm = hm*100;

  const bmi = (wkg>0 && hm>0) ? (wkg/(hm*hm)) : 0;

  // BMR (Mifflin–St Jeor)
  // Male: +5, Female: -161
  let bmr = 0;
  if(wkg>0 && hcm>0 && age>0){
    bmr = (10*wkg) + (6.25*hcm) - (5*age) + (sex==="male" ? 5 : -161);
  }

  const activity = n($("p_activity").value);
  const tdee = bmr>0 ? bmr*activity : 0;

  // Goal-based target calories
  const goal = $("p_goal").value;
  let targetCalories = tdee;
  if(goal==="loss") targetCalories = tdee - 500;
  if(goal==="gain") targetCalories = tdee + 400;

  // Protein range (g/kg). Show range + default reference set above minimum.
  const protMultMin = 1.5;
  const protMultMax = 2.0;
  const pMin = wkg>0 ? wkg*protMultMin : 0;
  const pMax = wkg>0 ? wkg*protMultMax : 0;

  // Default protein target: slightly above min (rounded)
  // Example: 161–214 → 200
  let pTargetAuto = 0;
  if(pMin>0){
    const ref = pMin + (pMax-pMin)*0.45; // above min, below max
    pTargetAuto = Math.round(ref/5)*5;
  }

  // Macro preset
  const preset = $("p_macroPreset").value;
  let fatPct = 0.30, carbPct = 0.40, protPct = 0.30;
  if(preset==="higherCarb"){ fatPct=0.25; carbPct=0.50; protPct=0.25; }
  if(preset==="higherProtein"){ fatPct=0.25; carbPct=0.35; protPct=0.40; }

  // Use protein target first, then distribute remaining calories to carbs/fats
  let pTarget = pTargetAuto;
  // if user already typed a value, keep it
  const userP = n($("p_targetProtein").value);
  if(userP>0) pTarget = userP;

  let kcalFromP = pTarget*4;
  let remaining = Math.max(0, targetCalories - kcalFromP);

  // Split remaining into carbs/fats by preset proportions (normalized to carb+fat)
  const cfTotal = carbPct + fatPct;
  const cKcal = remaining * (carbPct/cfTotal);
  const fKcal = remaining * (fatPct/cfTotal);

  let cTarget = Math.round((cKcal/4)/5)*5;
  let fTarget = Math.round((fKcal/9));

  // if user typed, keep
  const userC = n($("p_targetCarbs").value);
  const userF = n($("p_targetFats").value);
  if(userC>0) cTarget = userC;
  if(userF>0) fTarget = userF;

  // if user typed target calories, keep
  const userTC = n($("p_targetCalories").value);
  if(userTC>0) targetCalories = userTC;

  return {
    name: $("p_name").value || "",
    sex, age,
    heightUnit, weightUnit,
    height_cm: n($("p_height_cm").value),
    height_ft: n($("p_height_ft").value),
    height_in: n($("p_height_in").value),
    weight_kg: n($("p_weight_kg").value),
    weight_lbs: n($("p_weight_lbs").value),
    wkg, hm, bmi,
    bmr, tdee,
    goal,
    activity,
    dayStartHour: clamp(parseInt($("p_dayStartHour").value||"4",10),0,23),
    pMin, pMax,
    targetCalories,
    targetProtein: pTarget,
    targetCarbs: cTarget,
    targetFats: fTarget,
    macroPreset: preset
  };
}

function updateProfilePreview(){
  const p = getProfileDraft();
  $("p_bmiLine").textContent = p.bmi ? round2(p.bmi) : "—";
  $("p_bmrLine").textContent = p.bmr ? Math.round(p.bmr) + " kcal" : "—";
  $("p_tdeeLine").textContent = p.tdee ? Math.round(p.tdee) + " kcal" : "—";
  $("p_targetLine").textContent = "Target: " + (p.targetCalories ? Math.round(p.targetCalories) + " kcal" : "—");

  // Autofill targets if empty
  if(!n($("p_targetCalories").value) && p.targetCalories) $("p_targetCalories").value = Math.round(p.targetCalories);
  if(!n($("p_targetProtein").value) && p.targetProtein) $("p_targetProtein").value = p.targetProtein;
  if(!n($("p_targetCarbs").value) && p.targetCarbs) $("p_targetCarbs").value = p.targetCarbs;
  if(!n($("p_targetFats").value) && p.targetFats) $("p_targetFats").value = p.targetFats;

  refreshDashboard();
}

function saveProfile(){
  const p = getProfileDraft();
  localStorage.setItem(LS_PROFILE, JSON.stringify(p));
  $("profileSaveHint").style.display = "block";
  $("profileSavedNote").style.display = "block";
  refreshDashboard();
}

function loadProfile(){
  const raw = localStorage.getItem(LS_PROFILE);
  if(!raw) return null;
  try{
    const p = JSON.parse(raw);
    // Fill fields
    $("p_name").value = p.name || "";
    $("p_age").value = p.age || "";
    $("p_sex").value = p.sex || "male";

    $("p_height_unit").value = p.heightUnit || "cm";
    $("p_weight_unit").value = p.weightUnit || "kg";

    $("p_height_cm").value = p.height_cm || "";
    $("p_height_ft").value = p.height_ft || "";
    $("p_height_in").value = p.height_in || "";

    $("p_weight_kg").value = p.weight_kg || "";
    $("p_weight_lbs").value = p.weight_lbs || "";

    $("p_goal").value = p.goal || "loss";
    $("p_activity").value = (p.activity || 1.2).toString();
    $("p_dayStartHour").value = (p.dayStartHour ?? 4);

    $("p_macroPreset").value = p.macroPreset || "balanced";

    $("p_targetCalories").value = p.targetCalories ? Math.round(p.targetCalories) : "";
    $("p_targetProtein").value = p.targetProtein || "";
    $("p_targetCarbs").value = p.targetCarbs || "";
    $("p_targetFats").value = p.targetFats || "";

    $("profileSavedNote").style.display = "block";
    return p;
  }catch(e){ return null; }
}

function resetAll(){
  if(!confirm("Reset profile + today log on this device?")) return;
  localStorage.removeItem(LS_PROFILE);
  localStorage.removeItem(LS_LOG);
  location.reload();
}

/* =========================
   Profile UI toggles
========================= */
function toggleHeightUI(){
  const u = $("p_height_unit").value;
  $("height_cm_row").classList.toggle("hide", u!=="cm");
  $("height_ftin_row").classList.toggle("hide", u!=="ftin");
  updateProfilePreview();
}
function toggleWeightUI(){
  const u = $("p_weight_unit").value;
  $("weight_kg_row").classList.toggle("hide", u!=="kg");
  $("weight_lbs_row").classList.toggle("hide", u!=="lbs");
  updateProfilePreview();
}
$("p_height_unit").addEventListener("change", toggleHeightUI);
$("p_weight_unit").addEventListener("change", toggleWeightUI);

/* Live profile preview listeners */
[
 "p_name","p_age","p_height_cm","p_height_ft","p_height_in","p_weight_kg","p_weight_lbs",
 "p_goal","p_activity","p_dayStartHour","p_sex","p_macroPreset",
 "p_targetCalories","p_targetProtein","p_targetCarbs","p_targetFats"
].forEach(id=>{
  $(id).addEventListener("input", ()=>{ updateProfilePreview(); });
  $(id).addEventListener("change", ()=>{ updateProfilePreview(); });
});

/* =========================
   Log date logic (day starts at user hour)
========================= */
function getDefaultLogDate(profile){
  const now = new Date();
  const dayStart = profile?.dayStartHour ?? 4;
  const adjusted = new Date(now);
  if(now.getHours() < dayStart){
    adjusted.setDate(now.getDate()-1);
  }
  return adjusted.toISOString().slice(0,10);
}

/* =========================
   Food log calculations
========================= */
function computeItemFromPer100(grams, per100){
  const f = grams/100;
  return {
    P: per100.P*f,
    F: per100.F*f,
    C: per100.C*f,
    K: per100.K*f,
  };
}

function sumMacros(a,b){
  return {P:a.P+b.P, F:a.F+b.F, C:a.C+b.C, K:a.K+b.K};
}

function computeSectionTotals(){
  // Chicken
  let chicken = {P:0,F:0,C:0,K:0};
  chicken = sumMacros(chicken, computeItemFromPer100(n($("chicken_breast").value), FOOD.chicken_breast.per100));
  chicken = sumMacros(chicken, computeItemFromPer100(n($("chicken_thigh").value), FOOD.chicken_thigh.per100));
  chicken = sumMacros(chicken, computeItemFromPer100(n($("chicken_wings").value), FOOD.chicken_wings.per100));
  chicken = sumMacros(chicken, computeItemFromPer100(n($("chicken_drumsticks").value), FOOD.chicken_drumsticks.per100));

  // Other proteins
  let prot = {P:0,F:0,C:0,K:0};
  prot = sumMacros(prot, {P:n($("egg").value)*FOOD.egg.perUnit.P, F:n($("egg").value)*FOOD.egg.perUnit.F, C:n($("egg").value)*FOOD.egg.perUnit.C, K:n($("egg").value)*FOOD.egg.perUnit.K});
  prot = sumMacros(prot, {P:n($("protein_shake").value)*FOOD.protein_shake.perUnit.P, F:n($("protein_shake").value)*FOOD.protein_shake.perUnit.F, C:n($("protein_shake").value)*FOOD.protein_shake.perUnit.C, K:n($("protein_shake").value)*FOOD.protein_shake.perUnit.K});
  ["mutton","duck","pigeon","quail","beef","fish","shrimp","dried_fish"].forEach(k=>{
    prot = sumMacros(prot, computeItemFromPer100(n($(k).value), FOOD[k].per100));
  });

  // Carbs
  let carbs = {P:0,F:0,C:0,K:0};
  carbs = sumMacros(carbs, computeItemFromPer100(n($("rice_g").value), FOOD.rice_g.per100));
  carbs = sumMacros(carbs, {P:n($("rice_plate").value)*FOOD.rice_plate.perPlate.P, F:n($("rice_plate").value)*FOOD.rice_plate.perPlate.F, C:n($("rice_plate").value)*FOOD.rice_plate.perPlate.C, K:n($("rice_plate").value)*FOOD.rice_plate.perPlate.K});
  carbs = sumMacros(carbs, {P:n($("bread").value)*FOOD.bread.perUnit.P, F:n($("bread").value)*FOOD.bread.perUnit.F, C:n($("bread").value)*FOOD.bread.perUnit.C, K:n($("bread").value)*FOOD.bread.perUnit.K});
  carbs = sumMacros(carbs, {P:n($("roti").value)*FOOD.roti.perUnit.P, F:n($("roti").value)*FOOD.roti.perUnit.F, C:n($("roti").value)*FOOD.roti.perUnit.C, K:n($("roti").value)*FOOD.roti.perUnit.K});
  carbs = sumMacros(carbs, {P:n($("paratha").value)*FOOD.paratha.perUnit.P, F:n($("paratha").value)*FOOD.paratha.perUnit.F, C:n($("paratha").value)*FOOD.paratha.perUnit.C, K:n($("paratha").value)*FOOD.paratha.perUnit.K});
  carbs = sumMacros(carbs, computeItemFromPer100(n($("oats_g").value), FOOD.oats_g.per100));
  carbs = sumMacros(carbs, {P:n($("milk_tea").value)*FOOD.milk_tea.perUnit.P, F:n($("milk_tea").value)*FOOD.milk_tea.perUnit.F, C:n($("milk_tea").value)*FOOD.milk_tea.perUnit.C, K:n($("milk_tea").value)*FOOD.milk_tea.perUnit.K});
  // soda ml -> per100ml
  carbs = sumMacros(carbs, computeItemFromPer100(n($("carbonated_beverage_ml").value), {P:0,F:0,C:10.6,K:42}));

  // Veg
  let veg = {P:0,F:0,C:0,K:0};
  veg = sumMacros(veg, {P:n($("salad").value)*FOOD.salad.perUnit.P, F:n($("salad").value)*FOOD.salad.perUnit.F, C:n($("salad").value)*FOOD.salad.perUnit.C, K:n($("salad").value)*FOOD.salad.perUnit.K});
  veg = sumMacros(veg, {P:n($("vegetables").value)*FOOD.vegetables.perUnit.P, F:n($("vegetables").value)*FOOD.vegetables.perUnit.F, C:n($("vegetables").value)*FOOD.vegetables.perUnit.C, K:n($("vegetables").value)*FOOD.vegetables.perUnit.K});

  // Fats (multi)
  let fats = {P:0,F:0,C:0,K:0};
  OILS.forEach(o=>{
    if($(o.key+"_chk").checked){
      const qty = n($(o.key+"_qty").value);
      fats = sumMacros(fats, {P:o.perUnit.P*qty, F:o.perUnit.F*qty, C:o.perUnit.C*qty, K:o.perUnit.K*qty});
    }
  });

  // Snacks & Fruits
  let snack = {P:0,F:0,C:0,K:0};

  // Dates
  const datesType = $("dates_type").value;
  let gPer = n($("dates_gPer").value || 8);
  // Defaults:
  // Deglet Noor ~ 8g/date (common), Ajwa small per vendor: 9g, Medjool ~24g
  if(datesType==="deglet") gPer = n($("dates_gPer").value || 8);
  if(datesType==="ajwaSmall") gPer = n($("dates_gPer").value || 9);
  if(datesType==="medjool") gPer = n($("dates_gPer").value || 24);

  const dates_g = n($("dates_g").value);
  const dates_pcs = n($("dates_pcs").value);
  const totalDatesG = dates_g + (dates_pcs*gPer);

  // Use Deglet Noor style per date macro: 1 date ~ 23 kcal, carbs 6.23g, protein 0.2g, fat 0.03g
  // Scale per gram: 282 kcal /100g, carbs 75g/100g approx; keep consistent with fatsecret/USDA-like.
  const datesPer100 = {P:2.2, F:0.3, C:75.0, K:282};
  snack = sumMacros(snack, computeItemFromPer100(totalDatesG, datesPer100));

  // Peanut butter (tbsp)
  const pbTbsp = n($("peanut_butter_tbsp").value);
  snack = sumMacros(snack, {P:PB_PER_TBSP.P*pbTbsp, F:PB_PER_TBSP.F*pbTbsp, C:PB_PER_TBSP.C*pbTbsp, K:PB_PER_TBSP.K*pbTbsp});

  // Yogurt
  snack = sumMacros(snack, computeItemFromPer100(n($("yogurt_g").value), FOOD.yogurt_g.per100));

  // Chocolate/Cake
  snack = sumMacros(snack, {P:n($("chocolate_serv").value)*FOOD.chocolate_serv.perUnit.P, F:n($("chocolate_serv").value)*FOOD.chocolate_serv.perUnit.F, C:n($("chocolate_serv").value)*FOOD.chocolate_serv.perUnit.C, K:n($("chocolate_serv").value)*FOOD.chocolate_serv.perUnit.K});
  snack = sumMacros(snack, {P:n($("cake_serv").value)*FOOD.cake_serv.perUnit.P, F:n($("cake_serv").value)*FOOD.cake_serv.perUnit.F, C:n($("cake_serv").value)*FOOD.cake_serv.perUnit.C, K:n($("cake_serv").value)*FOOD.cake_serv.perUnit.K});

  // Nuts multi (grams)
  NUTS.forEach(nn=>{
    if($(nn.key+"_chk").checked){
      const qty = n($(nn.key+"_qty").value);
      snack = sumMacros(snack, computeItemFromPer100(qty, nn.per100));
    }
  });

  // Fruits multi (pcs/cups)
  FRUITS.forEach(fr=>{
    if($(fr.key+"_chk").checked){
      const qty = n($(fr.key+"_qty").value);
      snack = sumMacros(snack, {P:fr.perUnit.P*qty, F:fr.perUnit.F*qty, C:fr.perUnit.C*qty, K:fr.perUnit.K*qty});
    }
  });

  // Meals (plate portions)
  let meals = {P:0,F:0,C:0,K:0};
  MEALS.forEach(m=>{
    if($(m.key+"_chk").checked){
      const portion = n($(m.key+"_plate").value);
      meals = sumMacros(meals, {P:m.perPlate.P*portion, F:m.perPlate.F*portion, C:m.perPlate.C*portion, K:m.perPlate.K*portion});
    }
  });

  return { chicken, prot, carbs, veg, fats, snack, meals };
}

function updateSectionPreviews(sections){
  const setPrev = (prefix, t)=>{
    $(prefix+"P").textContent = round1(t.P);
    $(prefix+"F").textContent = round1(t.F);
    $(prefix+"C").textContent = round1(t.C);
    $(prefix+"K").textContent = Math.round(t.K);
  };
  setPrev("prevChicken", sections.chicken);
  setPrev("prevProtein", sections.prot);
  setPrev("prevCarb", sections.carbs);
  setPrev("prevVeg", sections.veg);
  setPrev("prevFat", sections.fats);
  setPrev("prevSnack", sections.snack);
  setPrev("prevMeals", sections.meals);
}

function computeTotals(){
  const s = computeSectionTotals();
  updateSectionPreviews(s);
  let total = {P:0,F:0,C:0,K:0};
  Object.values(s).forEach(t=>{ total = sumMacros(total,t); });
  return { sections:s, total };
}

/* =========================
   Chart
   (Fixed height; colors NOT blue)
========================= */
let macroChart = null;
function initChart(){
  const ctx = $("macroChart").getContext("2d");
  macroChart = new Chart(ctx, {
    type:"bar",
    data:{
      labels:["Protein (g)","Carbs (g)","Fat (g)"],
      datasets:[{
        label:"Macros",
        data:[0,0,0],
        backgroundColor:["#3ddc97","#ffcc66","#ff6b6b"], // green, amber, red (no blue)
        borderWidth:0
      }]
    },
    options:{
      responsive:true,
      maintainAspectRatio:false,
      animation:false,
      scales:{
        y:{ beginAtZero:true, ticks:{ color:"rgba(238,243,255,.8)" }, grid:{ color:"rgba(255,255,255,.10)" } },
        x:{ ticks:{ color:"rgba(238,243,255,.8)" }, grid:{ display:false } }
      },
      plugins:{
        legend:{ labels:{ color:"rgba(238,243,255,.85)" } }
      }
    }
  });
}
initChart();

/* =========================
   Daily log persistence
========================= */
function loadDailyLog(){
  const raw = localStorage.getItem(LS_LOG);
  if(!raw) return {};
  try{ return JSON.parse(raw); }catch(e){ return {}; }
}

function saveDailyLogAll(obj){
  localStorage.setItem(LS_LOG, JSON.stringify(obj));
}

function currentLogKey(profile){
  const dateStr = $("logDate").value || getDefaultLogDate(profile);
  return dateStr;
}

function collectFoodInputs(){
  const data = {};

  // All direct inputs
  const ids = [
    "chicken_breast","chicken_thigh","chicken_wings","chicken_drumsticks",
    "egg","protein_shake","mutton","duck","pigeon","quail","beef","fish","shrimp","dried_fish",
    "rice_g","rice_plate","bread","roti","paratha","oats_g","milk_tea","carbonated_beverage_ml",
    "salad","vegetables",
    "dates_g","dates_pcs","dates_type","dates_gPer",
    "peanut_butter_tbsp","yogurt_g","chocolate_serv","cake_serv"
  ];
  ids.forEach(id=> data[id] = $(id).value);

  // Multi lists
  OILS.forEach(o=>{
    data[o.key+"_chk"] = $(o.key+"_chk").checked;
    data[o.key+"_qty"] = $(o.key+"_qty").value;
  });
  NUTS.forEach(nn=>{
    data[nn.key+"_chk"] = $(nn.key+"_chk").checked;
    data[nn.key+"_qty"] = $(nn.key+"_qty").value;
  });
  FRUITS.forEach(fr=>{
    data[fr.key+"_chk"] = $(fr.key+"_chk").checked;
    data[fr.key+"_qty"] = $(fr.key+"_qty").value;
  });
  MEALS.forEach(m=>{
    data[m.key+"_chk"] = $(m.key+"_chk").checked;
    data[m.key+"_plate"] = $(m.key+"_plate").value;
  });

  // Meal type
  data.mealType = $("mealType").value;

  return data;
}

function applyFoodInputs(data){
  if(!data) return;

  Object.keys(data).forEach(k=>{
    const el = $(k);
    if(!el) return;
    if(el.type==="checkbox"){
      el.checked = !!data[k];
    }else{
      el.value = data[k];
    }
  });
}

function saveTodayLog(){
  const profile = loadProfile() || getProfileDraft();
  const key = currentLogKey(profile);
  const all = loadDailyLog();
  all[key] = {
    food: collectFoodInputs(),
    dashboard: {
      sleepHours: $("sleepHours").value,
      waterLiters: $("waterLiters").value,
      exerciseCals: $("exerciseCals").value,
      workoutType: $("workoutType").value,
      workoutMinutes: $("workoutMinutes").value,
      strengthSplit: $("strengthSplit").value,
      workoutNotes: $("workoutNotes")?.value || ""
    }
  };
  saveDailyLogAll(all);
  alert("Saved today log ✔️");
  refreshDashboard();
}

function clearTodayLog(){
  const profile = loadProfile() || getProfileDraft();
  const key = currentLogKey(profile);
  const all = loadDailyLog();
  delete all[key];
  saveDailyLogAll(all);
  // Clear UI
  document.querySelectorAll("#page-food input, #page-food select").forEach(el=>{
    if(el.id==="logDate" || el.id==="mealType") return;
    if(el.type==="checkbox") el.checked=false;
    else el.value="";
  });
  // restore defaults
  $("rice_plate").value = "0";
  $("dates_type").value = "deglet";
  $("dates_gPer").value = "8";
  MEALS.forEach(m=> $(m.key+"_plate").value = "0");
  refreshDashboard();
}

/* =========================
   Auto refresh pipeline
========================= */
let refreshTimer = null;
function scheduleRefresh(){
  clearTimeout(refreshTimer);
  refreshTimer = setTimeout(()=>refreshDashboard(), 60);
}

function refreshDashboard(){
  const profile = loadProfile() || getProfileDraft();

  // Ensure log date filled
  if(!$("logDate").value){
    $("logDate").value = getDefaultLogDate(profile);
  }

  // Load saved log for selected date
  const all = loadDailyLog();
  const key = currentLogKey(profile);
  if(all[key]?.food){
    applyFoodInputs(all[key].food);
  }

  // Compute totals from inputs
  const { total } = computeTotals();

  // Live totals (Food page right column)
  $("liveP").textContent = round1(total.P) + " g";
  $("liveC").textContent = round1(total.C) + " g";
  $("liveF").textContent = round1(total.F) + " g";
  $("liveK").textContent = Math.round(total.K) + " kcal";

  // Dashboard KPIs
  $("dashCalories").textContent = Math.round(total.K) + " kcal";
  $("dashProtein").textContent = round1(total.P) + " g";
  $("dashCarbs").textContent = round1(total.C);
  $("dashFat").textContent = round1(total.F);

  const tCal = n(profile.targetCalories);
  const tP = n(profile.targetProtein);
  const tC = n(profile.targetCarbs);
  const tF = n(profile.targetFats);

  $("dashCalTarget").textContent = tCal ? Math.round(tCal) : 0;
  $("dashProtTarget").textContent = tP ? tP : 0;
  $("dashCarbTarget").textContent = tC ? tC : 0;
  $("dashFatTarget").textContent = tF ? tF : 0;

  // Balance messages: clearly say over/under
  const calDiff = Math.round(total.K - tCal);
  const protDiff = Math.round(total.P - tP);

  const calEl = $("dashCalBalance");
  const calMsg = $("dashCalBalanceMsg");
  if(!tCal){
    calEl.textContent = "—";
    calMsg.textContent = "Set profile targets first.";
  }else{
    if(calDiff>0){
      calEl.textContent = "+" + calDiff + " kcal";
      calEl.className = "big accentBad";
      calMsg.textContent = "Over target (you ate more than target).";
    }else if(calDiff<0){
      calEl.textContent = calDiff + " kcal";
      calEl.className = "big accentGood";
      calMsg.textContent = "Under target (you ate less than target).";
    }else{
      calEl.textContent = "0 kcal";
      calEl.className = "big";
      calMsg.textContent = "Exactly on target.";
    }
  }

  const pEl = $("dashProtBalance");
  const pMsg = $("dashProtBalanceMsg");
  if(!tP){
    pEl.textContent = "—";
    pMsg.textContent = "Set protein target in Profile.";
  }else{
    if(protDiff>0){
      pEl.textContent = "+" + protDiff + " g";
      pEl.className = "big accentGood";
      pMsg.textContent = "Above target protein.";
    }else if(protDiff<0){
      pEl.textContent = protDiff + " g";
      pEl.className = "big accentBad";
      pMsg.textContent = "Below target protein.";
    }else{
      pEl.textContent = "0 g";
      pEl.className = "big";
      pMsg.textContent = "Exactly on target protein.";
    }
  }

  // Metrics line
  $("dashBMI").textContent = profile.bmi ? round2(profile.bmi) : "—";
  $("dashBMR").textContent = profile.bmr ? Math.round(profile.bmr) : "—";
  $("dashTDEE").textContent = profile.tdee ? Math.round(profile.tdee) : "—";

  // BMI range indicator
  updateBMIBar(profile.bmi);

  // Update chart
  if(macroChart){
    macroChart.data.datasets[0].data = [round1(total.P), round1(total.C), round1(total.F)];
    macroChart.update();
  }
}

function updateBMIBar(bmi){
  const fill = $("bmiFill");
  const pin = $("bmiPin");
  if(!bmi || bmi<=0){
    fill.style.width = "0%";
    pin.style.left = "0%";
    return;
  }
  // Map BMI 10..40 -> 0..100
  const pct = clamp(((bmi-10)/(40-10))*100, 0, 100);
  fill.style.width = pct + "%";
  pin.style.left = pct + "%";
  pin.textContent = "▲ You (" + round1(bmi) + ")";
}

/* =========================
   Workout dropdown behavior
========================= */
$("workoutType").addEventListener("change", ()=>{
  const isStrength = $("workoutType").value==="Strength";
  $("strengthSplitWrap").classList.toggle("hide", !isStrength);
});

/* =========================
   Food input listeners (auto-calc)
========================= */
function attachAutoListeners(){
  // All inputs in food page
  document.querySelectorAll("#page-food input, #page-food select").forEach(el=>{
    el.addEventListener("input", scheduleRefresh);
    el.addEventListener("change", scheduleRefresh);
  });
  // Dashboard inputs
  ["sleepHours","waterLiters","exerciseCals","workoutType","workoutMinutes","strengthSplit"].forEach(id=>{
    $(id).addEventListener("input", scheduleRefresh);
    $(id).addEventListener("change", scheduleRefresh);
  });
}
attachAutoListeners();

/* =========================
   PDF report
========================= */
async function savePDF(){
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ unit:"pt", format:"a4" });

  const profile = loadProfile() || getProfileDraft();
  const { total, sections } = computeTotals();

  const pageW = doc.internal.pageSize.getWidth();
  let y = 46;

  doc.setFont("helvetica","bold");
  doc.setFontSize(18);
  doc.text("Nowshad's Macro Calculator — Report", 40, y);
  y += 18;

  doc.setFont("helvetica","normal");
  doc.setFontSize(10);
  doc.text("Generated: " + new Date().toLocaleString(), 40, y);
  y += 20;

  // Profile block
  doc.setFont("helvetica","bold");
  doc.setFontSize(12);
  doc.text("Profile Summary", 40, y); y += 10;
  doc.setDrawColor(220); doc.setLineWidth(1);
  doc.line(40, y, pageW-40, y); y += 12;

  doc.setFont("helvetica","normal");
  doc.setFontSize(11);

  const lines = [
    `Name: ${profile.name || "—"}   |   Age: ${profile.age || "—"}   |   Sex: ${profile.sex || "—"}`,
    `BMI: ${profile.bmi ? round2(profile.bmi) : "—"} (Recommended 18.5–24.9)   |   BMR: ${profile.bmr ? Math.round(profile.bmr) : "—"} kcal   |   TDEE: ${profile.tdee ? Math.round(profile.tdee) : "—"} kcal`,
    `Goal: ${profile.goal || "—"}   |   Target Calories: ${profile.targetCalories ? Math.round(profile.targetCalories) : "—"} kcal`,
    `Targets — Protein: ${profile.targetProtein || "—"} g   |   Carbs: ${profile.targetCarbs || "—"} g   |   Fats: ${profile.targetFats || "—"} g`,
  ];
  lines.forEach(t=>{ doc.text(t, 40, y); y += 16; });

  y += 8;
  doc.setFont("helvetica","bold");
  doc.text("Today’s Totals", 40, y); y += 10;
  doc.setFont("helvetica","normal");
  doc.text(`Calories: ${Math.round(total.K)} kcal   |   Protein: ${round1(total.P)} g   |   Carbs: ${round1(total.C)} g   |   Fat: ${round1(total.F)} g`, 40, y);
  y += 18;

  // Balance
  const calDiff = profile.targetCalories ? Math.round(total.K - profile.targetCalories) : 0;
  const pDiff = profile.targetProtein ? Math.round(total.P - profile.targetProtein) : 0;
  const calMsg = profile.targetCalories ? (calDiff>0 ? `Over target by ${calDiff} kcal` : calDiff<0 ? `Under target by ${Math.abs(calDiff)} kcal` : "On target calories") : "Set calorie target in Profile";
  const pMsg = profile.targetProtein ? (pDiff>0 ? `Above protein target by ${pDiff} g` : pDiff<0 ? `Below protein target by ${Math.abs(pDiff)} g` : "On target protein") : "Set protein target in Profile";
  doc.text(`Calories balance: ${calMsg}`, 40, y); y += 14;
  doc.text(`Protein balance: ${pMsg}`, 40, y); y += 18;

  // Chart image
  doc.setFont("helvetica","bold");
  doc.text("Macro Chart", 40, y); y += 10;

  const canvas = $("macroChart");
  const imgData = canvas.toDataURL("image/png", 1.0);
  doc.addImage(imgData, "PNG", 40, y, pageW-80, 220);
  y += 235;

  // Section summary
  doc.setFont("helvetica","bold");
  doc.text("Section Breakdown", 40, y); y += 10;
  doc.setFont("helvetica","normal");
  const secRows = [
    ["Chicken", sections.chicken],
    ["Other proteins", sections.prot],
    ["Carbs", sections.carbs],
    ["Vegetables & salad", sections.veg],
    ["Fats", sections.fats],
    ["Snacks & Fruits", sections.snack],
    ["Meals", sections.meals],
  ];
  secRows.forEach(([name, t])=>{
    doc.text(`${name}:  P ${round1(t.P)}g  |  C ${round1(t.C)}g  |  F ${round1(t.F)}g  |  ${Math.round(t.K)} kcal`, 40, y);
    y += 14;
    if(y>760){ doc.addPage(); y=46; }
  });

  y += 10;
  doc.setFontSize(9);
  doc.text("Disclaimer: Nutrition values are estimates. For personalized advice consult a qualified nutritionist/doctor.", 40, y);

  doc.save("Nowshad_Macro_Report.pdf");
}

/* =========================
   Boot
========================= */
(function boot(){
  const p = loadProfile();
  toggleHeightUI();
  toggleWeightUI();
  updateProfilePreview();

  // Set default date and load log if exists
  $("logDate").value = getDefaultLogDate(p || getProfileDraft());
  refreshDashboard();

  // If profile exists, show note nicely
  if(p){
    $("profileSavedNote").style.display = "block";
  }
})();
</script>

</body>
</html>

