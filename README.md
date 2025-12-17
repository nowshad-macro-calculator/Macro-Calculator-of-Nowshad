
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Nowshad's Macro Calculator</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&display=swap" rel="stylesheet">

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <style>
    :root{
      --bg1:#051c46;
      --bg2:#0b4db0;
      --card:#ffffff;
      --text:#0b1220;
      --muted:#5b667a;
      --line: rgba(15, 23, 42, .12);
      --shadow: 0 18px 50px rgba(0,0,0,.18);
      --radius: 18px;

      --p:#06D6A0;   /* protein */
      --f:#EF476F;   /* fat */
      --c:#FFD166;   /* carbs */

      --good:#0a7a55;
      --bad:#b4233a;
      --accent:#0b4db0;
    }

    *{ box-sizing:border-box; }
    body{
      margin:0;
      font-family: "Poppins", system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color:#eaf2ff;
      min-height:100vh;
      padding: 14px;

      background:
        radial-gradient(1100px 650px at 18% 10%, rgba(255,209,102,.18), transparent 55%),
        radial-gradient(900px 520px at 85% 15%, rgba(6,214,160,.15), transparent 55%),
        linear-gradient(135deg, var(--bg1), var(--bg2));
    }

    .app{
      max-width: 1150px;
      margin:0 auto;
      background: rgba(255,255,255,.96);
      color: var(--text);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow:hidden;
    }

    .topbar{
      padding: 16px 16px 12px 16px;
      border-bottom: 1px solid var(--line);
      background: linear-gradient(135deg, rgba(255,209,102,.20), rgba(6,214,160,.12));
      display:flex;
      align-items:flex-end;
      justify-content:space-between;
      gap: 12px;
      flex-wrap: wrap;
    }
    .title{
      display:flex;
      flex-direction:column;
      gap: 6px;
      min-width: 240px;
    }
    h1{ margin:0; font-size: 20px; letter-spacing: .2px; color:#071734; }
    .subtitle{ margin:0; font-size: 13px; color: var(--muted); line-height: 1.45; }

    .nav{
      display:flex;
      gap: 8px;
      flex-wrap: wrap;
    }
    .tab{
      border: 1px solid rgba(15,23,42,.14);
      background: #fff;
      color:#071734;
      padding: 10px 12px;
      border-radius: 999px;
      cursor:pointer;
      font-weight: 800;
      font-size: 13px;
    }
    .tab.active{
      border-color: rgba(11,77,176,.25);
      background: rgba(11,77,176,.08);
      color:#071734;
    }

    .content{ padding: 14px; }
    @media (min-width: 920px){
      body{ padding: 24px; }
      .topbar{ padding: 20px 22px 14px 22px; }
      h1{ font-size: 22px; }
      .content{ padding: 18px 22px 22px 22px; }
    }

    .grid{
      display:grid;
      grid-template-columns: 1fr;
      gap: 12px;
    }
    @media (min-width: 920px){
      .grid{ grid-template-columns: 1fr 1fr; gap: 14px; }
    }

    .card{
      background: var(--card);
      border: 1px solid var(--line);
      border-radius: 16px;
      padding: 12px;
      min-width: 0;
    }

    .card h2{
      margin: 0 0 10px 0;
      font-size: 14px;
      color:#071734;
      font-weight: 900;
      letter-spacing: .2px;
    }

    label{
      display:block;
      font-size: 12px;
      color: var(--muted);
      font-weight: 800;
      margin-bottom: 6px;
    }

    input, select, button, textarea{
      width:100%;
      padding: 10px 12px;
      border-radius: 12px;
      border: 1px solid rgba(15,23,42,.16);
      font-size: 14px;
      outline:none;
      background:#fff;
      color: var(--text);
      max-width: 100%;
    }

    .row{
      display:grid;
      grid-template-columns: 1fr;
      gap: 10px;
      margin-top: 10px;
    }
    @media (min-width: 640px){
      .row.two{ grid-template-columns: 1fr 1fr; }
      .row.three{ grid-template-columns: 1fr 1fr 1fr; }
    }

    .kpis{
      display:grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
    }
    @media (min-width: 920px){
      .kpis{ grid-template-columns: repeat(6, 1fr); }
    }
    .kpi{
      border-radius: 14px;
      border: 1px solid var(--line);
      padding: 12px;
      background: #fff;
      min-width: 0;
    }
    .kpi .t{ font-size: 11px; color: var(--muted); font-weight: 800; }
    .kpi .v{ margin-top: 6px; font-size: 17px; font-weight: 900; color:#071734; }
    .kpi .v small{ font-weight: 800; color: var(--muted); }

    .badge{
      margin-top: 10px;
      padding: 12px;
      border-radius: 14px;
      border: 1px solid rgba(11,77,176,.18);
      background: rgba(11,77,176,.06);
      color:#071734;
      font-size: 12px;
      line-height: 1.5;
    }

    .bmiBar{
      height: 14px;
      border-radius: 999px;
      border: 1px solid rgba(15,23,42,.16);
      background:
        linear-gradient(to right,
          #06d6a0 0%, #06d6a0 45%,
          #ffd166 45%, #ffd166 70%,
          #ef476f 70%, #ef476f 100%
        );
      position:relative;
      overflow:hidden;
      margin-top: 10px;
    }
    .bmiMarker{
      position:absolute; top:-6px;
      width:2px; height:26px;
      background:#071734;
      box-shadow: 0 0 0 2px rgba(255,255,255,.9);
    }
    .legend{
      display:flex;
      justify-content:space-between;
      gap: 8px;
      flex-wrap: wrap;
      margin-top: 6px;
      font-size: 11px;
      color: var(--muted);
    }

    .chartBox{
      height: 260px;
      position: relative;
    }

    .preview{
      margin-top: 10px;
      padding: 10px 12px;
      border-radius: 14px;
      border: 1px dashed rgba(15,23,42,.18);
      background: rgba(255,209,102,.08);
      color:#071734;
      font-size: 12px;
      line-height: 1.55;
    }

    .actions{
      display:flex;
      gap: 10px;
      flex-wrap: wrap;
      margin-top: 12px;
    }
    .btn{
      border:none;
      cursor:pointer;
      font-weight: 900;
      letter-spacing: .2px;
      padding: 11px 14px;
      border-radius: 12px;
      width: auto;
      min-width: 160px;
    }
    .btn.primary{
      background: linear-gradient(135deg, #0b4db0, #07367f);
      color:#fff;
    }
    .btn.secondary{
      background:#fff;
      border:1px solid rgba(15,23,42,.16);
      color:#071734;
    }
    .btn.danger{
      background: rgba(239,71,111,.10);
      border: 1px solid rgba(239,71,111,.30);
      color:#071734;
    }

    .dyn{
      display:grid;
      gap: 10px;
      margin-top: 10px;
    }
    .dynRow{
      display:grid;
      grid-template-columns: 1.3fr .9fr .9fr auto;
      gap: 8px;
      align-items:end;
    }
    @media (max-width: 680px){
      .dynRow{ grid-template-columns: 1fr 1fr; }
      .dynRow .span2{ grid-column: span 2; }
      .dynRow .btnMini{ grid-column: span 2; }
    }
    .removeBtn{
      padding: 10px 10px;
      border-radius: 12px;
      border: 1px solid rgba(239,71,111,.35);
      background: rgba(239,71,111,.08);
      color:#071734;
      cursor:pointer;
      font-weight: 900;
      min-width: 44px;
    }
    .btnMini{
      padding: 10px 12px;
      border-radius: 12px;
      font-weight: 900;
      cursor:pointer;
      border: 1px solid rgba(15,23,42,.16);
      background:#fff;
      color:#071734;
      width: 100%;
    }

    .split{
      display:flex;
      gap: 10px;
      flex-wrap: wrap;
      align-items:center;
      justify-content: space-between;
      margin-top: 6px;
    }
    .pill{
      padding: 8px 10px;
      border-radius: 999px;
      border: 1px solid rgba(15,23,42,.12);
      background: rgba(6,214,160,.08);
      color:#071734;
      font-size: 12px;
      font-weight: 900;
    }
    .pill.red{ background: rgba(239,71,111,.10); }
    .pill.blue{ background: rgba(11,77,176,.08); }

    .footer{
      padding: 12px 14px 16px 14px;
      border-top: 1px solid var(--line);
      background: rgba(11,77,176,.04);
      color: var(--muted);
      font-size: 12px;
      line-height: 1.45;
    }
    @media (min-width: 920px){
      .footer{ padding: 14px 22px 18px 22px; }
    }

    .hidden{ display:none !important; }
  </style>
</head>

<body>
  <div class="app">
    <div class="topbar">
      <div class="title">
        <h1>Nowshad's Macro Calculator</h1>
        <p class="subtitle">Profile is saved. Log foods + sleep + water + exercise daily. Dashboard shows your totals and balances.</p>
      </div>
      <div class="nav">
        <button class="tab active" data-page="home">Home / Dashboard</button>
        <button class="tab" data-page="profile">Profile</button>
        <button class="tab" data-page="food">Food & Lifestyle</button>
      </div>
    </div>

    <div class="content">
      <!-- HOME / DASHBOARD -->
      <section id="page-home">
        <div class="card">
          <h2>Today’s Dashboard</h2>

          <div class="row two">
            <div>
              <label>Date</label>
              <input id="todayDate" type="text" readonly />
            </div>
            <div>
              <label>Goal</label>
              <div class="split">
                <span class="pill blue" id="goalPill">Weight loss</span>
                <span class="pill" id="profilePill">Profile: Not set</span>
              </div>
            </div>
          </div>

          <div class="kpis" style="margin-top:12px;">
            <div class="kpi">
              <div class="t">Calories eaten</div>
              <div class="v"><span id="kcalIn">0</span> <small>kcal</small></div>
            </div>
            <div class="kpi">
              <div class="t">Exercise burned</div>
              <div class="v"><span id="kcalOut">0</span> <small>kcal</small></div>
            </div>
            <div class="kpi">
              <div class="t">Net calories</div>
              <div class="v"><span id="kcalNet">0</span> <small>kcal</small></div>
            </div>
            <div class="kpi">
              <div class="t">Target calories</div>
              <div class="v"><span id="kcalTarget">0</span> <small>kcal</small></div>
            </div>
            <div class="kpi">
              <div class="t">Calories balance</div>
              <div class="v" id="kcalBalanceBox"><span id="kcalBalance">0</span> <small>kcal</small></div>
            </div>
            <div class="kpi">
              <div class="t">Protein balance</div>
              <div class="v" id="protBalanceBox"><span id="protBalance">0</span> <small>g</small></div>
            </div>
          </div>

          <div class="row two" style="margin-top:12px;">
            <div class="card" style="padding:12px; border-radius:14px;">
              <h2 style="margin-bottom:6px;">Macro chart</h2>
              <div class="chartBox">
                <canvas id="macroChart"></canvas>
              </div>
            </div>
            <div class="card" style="padding:12px; border-radius:14px;">
              <h2 style="margin-bottom:6px;">Sleep + Water + Activity</h2>
              <div class="badge" id="lifestyleSummary">
                Add sleep, water, and exercise on the “Food & Lifestyle” page to see recommendations here.
              </div>
              <div class="badge" style="margin-top:10px;">
                Sleep guidance is age-based (CDC). :contentReference[oaicite:4]{index=4}<br/>
                Water guidance uses general Adequate Intake references (National Academies / Mayo / Harvard). :contentReference[oaicite:5]{index=5}<br/>
                Activity guidance references WHO/AHA/CDC weekly recommendations. :contentReference[oaicite:6]{index=6}
              </div>
            </div>
          </div>

          <div class="actions">
            <button class="btn primary" id="btnPDF">Export Detailed Report (PDF)</button>
            <button class="btn secondary" id="btnClearToday">Clear Today’s Log</button>
            <button class="btn danger" id="btnResetAll">Reset Profile + All Data</button>
          </div>
        </div>
      </section>

      <!-- PROFILE -->
      <section id="page-profile" class="hidden">
        <div class="grid">
          <div class="card">
            <h2>User Profile (saved)</h2>

            <div class="row two">
              <div>
                <label>Name</label>
                <input id="p_name" type="text" placeholder="Enter name">
              </div>
              <div>
                <label>Gender</label>
                <select id="p_gender">
                  <option value="male">Male</option>
                  <option value="female">Female</option>
                </select>
              </div>
            </div>

            <div class="row two">
              <div>
                <label>Age</label>
                <input id="p_age" type="number" min="1" placeholder="e.g., 30">
              </div>
              <div>
                <label>Goal (default = weight loss)</label>
                <select id="p_goal">
                  <option value="loss" selected>Weight loss</option>
                  <option value="maint">Maintenance</option>
                  <option value="gain">Weight gain</option>
                </select>
              </div>
            </div>

            <div class="row two">
              <div>
                <label>Height unit</label>
                <select id="p_h_unit">
                  <option value="cm">cm</option>
                  <option value="ftin">ft/in</option>
                </select>
              </div>
              <div>
                <label>Weight unit</label>
                <select id="p_w_unit">
                  <option value="kg">kg</option>
                  <option value="lbs">lbs</option>
                </select>
              </div>
            </div>

            <div class="row two" id="h_cm_row">
              <div>
                <label>Height (cm)</label>
                <input id="p_h_cm" type="number" min="1" placeholder="e.g., 170">
              </div>
              <div></div>
            </div>

            <div class="row two hidden" id="h_ftin_row">
              <div>
                <label>Height (ft)</label>
                <input id="p_h_ft" type="number" min="0" placeholder="Feet">
              </div>
              <div>
                <label>Height (in)</label>
                <input id="p_h_in" type="number" min="0" placeholder="Inches">
              </div>
            </div>

            <div class="row two" id="w_kg_row">
              <div>
                <label>Weight (kg)</label>
                <input id="p_w_kg" type="number" min="1" placeholder="e.g., 70">
              </div>
              <div></div>
            </div>

            <div class="row two hidden" id="w_lbs_row">
              <div>
                <label>Weight (lbs)</label>
                <input id="p_w_lbs" type="number" min="1" placeholder="e.g., 154">
              </div>
              <div></div>
            </div>

            <div class="row two">
              <div>
                <label>Activity level (TDEE multiplier)</label>
                <select id="p_activity">
                  <option value="1.2">Sedentary</option>
                  <option value="1.375">Light (1–3 days/week)</option>
                  <option value="1.55">Moderate (4–5 days/week)</option>
                  <option value="1.725">Very Active (6–7 days/week)</option>
                  <option value="1.9">Extra Active</option>
                </select>
                <div class="badge" style="margin-top:10px;">
                  Activity levels and TDEE concepts are aligned with common TDEE calculators. :contentReference[oaicite:7]{index=7}
                </div>
              </div>
              <div>
                <label>Target protein (g) — auto-set above minimum (editable)</label>
                <input id="p_target_protein" type="number" min="0" placeholder="Auto-set after weight">
              </div>
            </div>

            <div class="actions">
              <button class="btn primary" id="btnSaveProfile">Save Profile</button>
              <button class="btn secondary" id="btnRecalcProfile">Recalculate BMI/BMR/TDEE</button>
            </div>
          </div>

          <div class="card">
            <h2>Metrics (BMI • BMR • TDEE)</h2>

            <div class="kpis">
              <div class="kpi">
                <div class="t">BMI</div>
                <div class="v"><span id="m_bmi">0</span></div>
              </div>
              <div class="kpi">
                <div class="t">BMR</div>
                <div class="v"><span id="m_bmr">0</span> <small>kcal</small></div>
              </div>
              <div class="kpi">
                <div class="t">TDEE</div>
                <div class="v"><span id="m_tdee">0</span> <small>kcal</small></div>
              </div>
              <div class="kpi">
                <div class="t">Target calories</div>
                <div class="v"><span id="m_target">0</span> <small>kcal</small></div>
              </div>
              <div class="kpi">
                <div class="t">Recommended protein</div>
                <div class="v"><span id="m_protRange">0</span></div>
              </div>
              <div class="kpi">
                <div class="t">Profile status</div>
                <div class="v" id="m_profileStatus">Not saved</div>
              </div>
            </div>

            <div class="bmiBar">
              <div class="bmiMarker" id="bmiMarker" style="left:0%;"></div>
            </div>
            <div class="legend">
              <span><b>Under</b> &lt;18.5</span>
              <span><b>Normal</b> 18.5–24.9</span>
              <span><b>Over</b> 25–29.9</span>
              <span><b>Obese</b> ≥30</span>
            </div>

            <div class="badge" id="bmiMsg">
              Enter and save your profile to see BMI/BMR/TDEE and a smarter daily dashboard.
            </div>

            <div class="badge" style="margin-top:10px;">
              BMR uses Mifflin–St Jeor (gender-adjusted). TDEE = BMR × activity multiplier. :contentReference[oaicite:8]{index=8}
            </div>
          </div>
        </div>
      </section>

      <!-- FOOD & LIFESTYLE -->
      <section id="page-food" class="hidden">
        <div class="grid">
          <div class="card">
            <h2>Food Log (today)</h2>
            <div class="badge">
              Add foods under each category. Totals update instantly and are saved for today.
            </div>

            <!-- PROTEIN -->
            <div class="card" style="margin-top:12px; border-radius:14px;">
              <h2>Protein</h2>
              <button class="btnMini" type="button" id="addProteinRow">+ Add protein item</button>
              <div id="proteinRows" class="dyn"></div>
              <div class="preview"><b>Preview:</b> P <span id="pvP">0</span>g • F <span id="pvF">0</span>g • C <span id="pvC">0</span>g • kcal <span id="pvK">0</span></div>
            </div>

            <!-- CARBS -->
            <div class="card" style="margin-top:12px; border-radius:14px;">
              <h2>Carbs</h2>
              <button class="btnMini" type="button" id="addCarbRow">+ Add carb item</button>
              <div id="carbRows" class="dyn"></div>
              <div class="preview"><b>Preview:</b> P <span id="cvP">0</span>g • F <span id="cvF">0</span>g • C <span id="cvC">0</span>g • kcal <span id="cvK">0</span></div>
            </div>

            <!-- FATS -->
            <div class="card" style="margin-top:12px; border-radius:14px;">
              <h2>Fats (Oils/Butter/Ghee)</h2>
              <button class="btnMini" type="button" id="addFatRow">+ Add fat item</button>
              <div id="fatRows" class="dyn"></div>
              <div class="preview"><b>Preview:</b> P <span id="fvP">0</span>g • F <span id="fvF">0</span>g • C <span id="fvC">0</span>g • kcal <span id="fvK">0</span></div>
            </div>

            <!-- SNACKS -->
            <div class="card" style="margin-top:12px; border-radius:14px;">
              <h2>Snacks (includes Peanut Butter)</h2>
              <button class="btnMini" type="button" id="addSnackRow">+ Add snack item</button>
              <div id="snackRows" class="dyn"></div>
              <div class="preview"><b>Preview:</b> P <span id="svP">0</span>g • F <span id="svF">0</span>g • C <span id="svC">0</span>g • kcal <span id="svK">0</span></div>
            </div>

            <!-- MEALS -->
            <div class="card" style="margin-top:12px; border-radius:14px;">
              <h2>Meals</h2>
              <button class="btnMini" type="button" id="addMealRow">+ Add meal item</button>
              <div id="mealRows" class="dyn"></div>
              <div class="preview"><b>Preview:</b> P <span id="mvP">0</span>g • F <span id="mvF">0</span>g • C <span id="mvC">0</span>g • kcal <span id="mvK">0</span></div>
            </div>
          </div>

          <div class="card">
            <h2>Lifestyle (today)</h2>

            <div class="row two">
              <div>
                <label>Sleep (hours)</label>
                <input id="sleepHours" type="number" min="0" step="0.25" placeholder="e.g., 7.5">
              </div>
              <div>
                <label>Water (liters)</label>
                <input id="waterLiters" type="number" min="0" step="0.1" placeholder="e.g., 3.0">
              </div>
            </div>

            <div class="badge" id="sleepWaterRec">
              Recommendations will appear after profile is saved (age + gender). Sleep guidance: CDC. :contentReference[oaicite:9]{index=9} Water: National Academies/Mayo/Harvard. :contentReference[oaicite:10]{index=10}
            </div>

            <div class="card" style="margin-top:12px; border-radius:14px;">
              <h2>Exercise (today)</h2>
              <button class="btnMini" type="button" id="addExerciseRow">+ Add workout</button>
              <div id="exerciseRows" class="dyn"></div>

              <div class="badge" style="margin-top:10px;">
                Weekly target (adults): at least 150 min moderate OR 75 min vigorous; more for extra benefits. :contentReference[oaicite:11]{index=11}
              </div>
            </div>

            <div class="actions">
              <button class="btn primary" id="btnSaveToday">Save Today</button>
              <button class="btn secondary" id="btnRecalcNow">Recalculate Totals</button>
            </div>

            <div class="badge" style="margin-top:10px;">
              Tip: “Calories burned” is user-entered because watches/treadmills vary widely. Your dashboard uses it to compute net calories.
            </div>
          </div>
        </div>
      </section>
    </div>

    <div class="footer">
      <b>Disclaimer:</b> Estimates only. For medical conditions, consult a clinician or registered dietitian.
    </div>
  </div>

<script>
/* =========================
   Storage model
========================= */
const LS_PROFILE = "nm_profile_v2";
const LS_DAILY_PREFIX = "nm_daily_v2_"; // + YYYY-MM-DD

function todayKey(){
  const d = new Date();
  const yyyy = d.getFullYear();
  const mm = String(d.getMonth()+1).padStart(2,'0');
  const dd = String(d.getDate()).padStart(2,'0');
  return `${yyyy}-${mm}-${dd}`;
}
function dailyKey(dateStr){ return LS_DAILY_PREFIX + dateStr; }

function loadProfile(){
  try{ return JSON.parse(localStorage.getItem(LS_PROFILE) || "null"); }catch(e){ return null; }
}
function saveProfile(p){ localStorage.setItem(LS_PROFILE, JSON.stringify(p)); }

function loadDaily(dateStr){
  try{ return JSON.parse(localStorage.getItem(dailyKey(dateStr)) || "null"); }catch(e){ return null; }
}
function saveDaily(dateStr, d){ localStorage.setItem(dailyKey(dateStr), JSON.stringify(d)); }
function clearDaily(dateStr){ localStorage.removeItem(dailyKey(dateStr)); }

function hardResetAll(){
  localStorage.removeItem(LS_PROFILE);
  Object.keys(localStorage).forEach(k=>{
    if(k.startsWith(LS_DAILY_PREFIX)) localStorage.removeItem(k);
  });
}

/* =========================
   Helpers
========================= */
function clamp0(x){ return x < 0 ? 0 : x; }
function round2(x){ return (Math.round(x*100)/100).toFixed(2); }
function round0(x){ return (Math.round(x)).toString(); }

function el(id){ return document.getElementById(id); }

function numFromInput(inputEl){
  const v = parseFloat(inputEl.value);
  return isNaN(v) ? 0 : v;
}

function weightKgFromProfile(p){
  if(!p) return 0;
  if(p.w_unit === "kg") return p.w_kg || 0;
  return (p.w_lbs || 0) * 0.453592;
}
function heightMFromProfile(p){
  if(!p) return 0;
  if(p.h_unit === "cm") return (p.h_cm || 0) / 100;
  const ft = p.h_ft || 0;
  const inch = p.h_in || 0;
  return (ft * 0.3048) + (inch * 0.0254);
}

/* =========================
   Metrics (BMI, BMR, TDEE)
========================= */
function calcBMI(p){
  const w = weightKgFromProfile(p);
  const h = heightMFromProfile(p);
  if(w<=0 || h<=0) return 0;
  return w / (h*h);
}

// Mifflin–St Jeor:
// men:    BMR = 10w + 6.25h - 5a + 5
// women:  BMR = 10w + 6.25h - 5a - 161
function calcBMR(p){
  const w = weightKgFromProfile(p);
  const h_cm = heightMFromProfile(p) * 100;
  const age = p?.age || 0;
  if(w<=0 || h_cm<=0 || age<=0) return 0;
  const base = (10*w) + (6.25*h_cm) - (5*age);
  return base + (p.gender === "female" ? -161 : 5);
}
function calcTDEE(p){
  const bmr = calcBMR(p);
  const mult = parseFloat(p.activity || "1.2");
  if(bmr<=0) return 0;
  return bmr * mult;
}
function targetCaloriesFromGoal(tdee, goal){
  if(!tdee) return 0;
  if(goal === "loss") return tdee - 500;   // simple, consistent
  if(goal === "gain") return tdee + 400;
  return tdee;
}
function proteinRange(p){
  const w = weightKgFromProfile(p);
  if(w<=0) return {min:0,max:0};
  return {min: w*1.5, max: w*2.0};
}
function suggestedTargetProtein(p){
  const {min, max} = proteinRange(p);
  if(min<=0 || max<=0) return 0;
  // above minimum (closer to upper-mid): min + 60% of range, rounded up to nearest 10
  const suggested = min + 0.60*(max-min);
  return Math.ceil(suggested/10)*10;
}

/* =========================
   Food DB (approx values)
   You can refine later; structure is ready.
========================= */
function mk(p,c,f,k){ return {p,c,f,k}; }
function sumMacro(a,b){ return {p:a.p+b.p, c:a.c+b.c, f:a.f+b.f, k:a.k+b.k}; }
function kcalFromPCF(p,c,f){ return (p*4)+(c*4)+(f*9); }

// Units:
// - grams: per 100g
// - count: per item
// - tbsp: per tbsp
// - cup: per cup
// - piece: per piece
const FOOD = {
  // Protein (some from your initial logic)
  protein: [
    {key:"chicken_breast", label:"Chicken Breast", unit:"grams", per100: mk(31,0,0, 31*4)},
    {key:"chicken_thigh", label:"Chicken Thigh", unit:"grams", per100: mk(26,0,8, kcalFromPCF(26,0,8))},
    {key:"chicken_wings", label:"Chicken Wings", unit:"grams", per100: mk(30,0,0, 30*4)},
    {key:"chicken_drumsticks", label:"Chicken Drumsticks", unit:"grams", per100: mk(28,0,10, kcalFromPCF(28,0,10))},
    {key:"mutton", label:"Mutton", unit:"grams", per100: mk(25,0,20, kcalFromPCF(25,0,20))},
    {key:"beef", label:"Beef", unit:"grams", per100: mk(26,0,15, kcalFromPCF(26,0,15))},
    {key:"fish", label:"Fish", unit:"grams", per100: mk(22,0,5, kcalFromPCF(22,0,5))},
    {key:"shrimp", label:"Shrimp", unit:"grams", per100: mk(24,0,2, kcalFromPCF(24,0,2))},
    {key:"dried_fish", label:"Dried Fish/Bombay Duck", unit:"grams", per100: mk(60,0,5, kcalFromPCF(60,0,5))},
    {key:"egg", label:"Egg", unit:"count", per1: mk(6,0.6,5, kcalFromPCF(6,0.6,5))},
    {key:"protein_shake", label:"Protein Shake (scoop)", unit:"count", per1: mk(25,3,2, kcalFromPCF(25,3,2))},
    {key:"yogurt", label:"Yogurt/Curd", unit:"grams", per100: mk(3.47,4.66,3.25,61)}
  ],

  // Carbs
  carbs: [
    {key:"rice_cooked", label:"Rice (cooked)", unit:"grams", per100: mk(2.7,28,0.3,130)},
    {key:"roti", label:"Roti", unit:"piece", per1: mk(3,18,1.5, kcalFromPCF(3,18,1.5))},
    {key:"paratha", label:"Paratha", unit:"piece", per1: mk(4,30,10, kcalFromPCF(4,30,10))},
    {key:"oats", label:"Oats (dry)", unit:"grams", per100: mk(13.2,67.7,6.5,389)},
    {key:"bread", label:"Bread (slice)", unit:"count", per1: mk(3,15,1.2, kcalFromPCF(3,15,1.2))},
    {key:"vegetables", label:"Vegetables", unit:"cup", per1: mk(2,5,0.2, kcalFromPCF(2,5,0.2))},
    {key:"salad", label:"Salad", unit:"cup", per1: mk(0.5,2,0.1, kcalFromPCF(0.5,2,0.1))}
  ],

  // Fats (tbsp)
  fats: [
    {key:"olive_oil", label:"Olive Oil", unit:"tbsp", per1: mk(0,0,13.5,119)},
    {key:"soybean_oil", label:"Soybean Oil", unit:"tbsp", per1: mk(0,0,13.6,120)},
    {key:"butter", label:"Butter", unit:"tbsp", per1: mk(0.12,0.01,11.36,100)},
    {key:"ghee", label:"Ghee", unit:"tbsp", per1: mk(0.04,0,12.7,112)}
  ],

  // Snacks
  snacks: [
    {key:"peanut_butter", label:"Peanut Butter", unit:"tbsp", per1: mk(3.9,2.7,8.7,97)},
    {key:"dates_generic", label:"Dates (generic)", unit:"piece", per1: mk(0.2,6,0,23)}, // reference-style
    {key:"almonds", label:"Almonds", unit:"grams", per100: mk(21.2,21.6,49.9,579)},
    {key:"peanuts", label:"Peanuts", unit:"grams", per100: mk(25.8,16.1,49.2,567)},
    {key:"cashews", label:"Cashews", unit:"grams", per100: mk(18.22,30.19,43.85,553)},
    {key:"banana", label:"Banana", unit:"piece", per1: mk(1.3,27,0.4,105)},
    {key:"apple", label:"Apple", unit:"piece", per1: mk(0.5,25,0.3,95)},
    {key:"orange", label:"Orange", unit:"piece", per1: mk(1.2,15,0.2,62)},
    {key:"milk_tea", label:"Milk Tea", unit:"cup", per1: mk(0,10,0,40)},
    {key:"chocolate", label:"Chocolate (cube)", unit:"count", per1: mk(1,15,5, kcalFromPCF(1,15,5))},
    {key:"cake", label:"Cake (slice)", unit:"count", per1: mk(3,30,10, kcalFromPCF(3,30,10))},
    {key:"soda", label:"Carbonated Beverage (can)", unit:"count", per1: mk(0,40,0,160)}
  ],

  // Meals (approx per 100g)
  meals: [
    {key:"biryani", label:"Chicken Biryani", unit:"grams", per100: mk(4.54,13.73,2.81,99.4)},
    {key:"khichuri", label:"Khichuri", unit:"grams", per100: mk(1.75,9.79,0.51,50)},
    {key:"fried_rice", label:"Fried Rice", unit:"grams", per100: mk(3.56,11.91,3.53,95.1)}
  ]
};

/* =========================
   Dynamic row builders
========================= */
function makeSelect(options, value){
  const s = document.createElement("select");
  options.forEach(o=>{
    const opt = document.createElement("option");
    opt.value = o.value; opt.textContent = o.label;
    s.appendChild(opt);
  });
  if(value) s.value = value;
  return s;
}

function addFoodRow(containerId, categoryKey, preset){
  const container = el(containerId);
  const wrap = document.createElement("div");
  wrap.className = "dynRow";

  const items = FOOD[categoryKey];
  const select = makeSelect(items.map(it=>({value:it.key, label:`${it.label} (${it.unit})`})), preset || items[0].key);

  const amount = document.createElement("input");
  amount.type = "number";
  amount.min = "0";
  amount.step = "0.1";
  amount.placeholder = "Amount";

  const note = document.createElement("input");
  note.type = "text";
  note.placeholder = "Optional note (e.g., brand/size)";
  note.className = "span2";

  const remove = document.createElement("button");
  remove.type="button";
  remove.className="removeBtn";
  remove.textContent="×";
  remove.onclick = ()=>{ wrap.remove(); scheduleAll(); };

  const box1 = document.createElement("div");
  box1.className="span2";
  box1.innerHTML = "<label>Item</label>";
  box1.appendChild(select);

  const box2 = document.createElement("div");
  box2.innerHTML = "<label>Amount</label>";
  box2.appendChild(amount);

  const box3 = document.createElement("div");
  box3.innerHTML = "<label>Note</label>";
  box3.appendChild(note);

  wrap.appendChild(box1);
  wrap.appendChild(box2);
  wrap.appendChild(box3);
  wrap.appendChild(remove);

  [select, amount, note].forEach(x=>{
    x.addEventListener("input", scheduleAll);
    x.addEventListener("change", scheduleAll);
  });

  container.appendChild(wrap);
}

function addExerciseRow(){
  const container = el("exerciseRows");
  const wrap = document.createElement("div");
  wrap.className="dynRow";

  const type = document.createElement("input");
  type.type="text"; type.placeholder="Workout type (e.g., cardio/weights)"; type.className="span2";

  const mins = document.createElement("input");
  mins.type="number"; mins.min="0"; mins.step="1"; mins.placeholder="Minutes";

  const kcal = document.createElement("input");
  kcal.type="number"; kcal.min="0"; kcal.step="1"; kcal.placeholder="Calories burned";

  const remove = document.createElement("button");
  remove.type="button"; remove.className="removeBtn"; remove.textContent="×";
  remove.onclick = ()=>{ wrap.remove(); scheduleAll(); };

  const box1 = document.createElement("div");
  box1.className="span2";
  box1.innerHTML="<label>Workout type</label>";
  box1.appendChild(type);

  const box2 = document.createElement("div");
  box2.innerHTML="<label>Minutes</label>";
  box2.appendChild(mins);

  const box3 = document.createElement("div");
  box3.innerHTML="<label>Calories burned</label>";
  box3.appendChild(kcal);

  wrap.appendChild(box1);
  wrap.appendChild(box2);
  wrap.appendChild(box3);
  wrap.appendChild(remove);

  [type, mins, kcal].forEach(x=>{
    x.addEventListener("input", scheduleAll);
    x.addEventListener("change", scheduleAll);
  });

  container.appendChild(wrap);
}

/* =========================
   Compute totals from rows
========================= */
function itemByKey(categoryKey, key){
  return FOOD[categoryKey].find(x=>x.key===key);
}
function macroFromItem(item, amount){
  const a = amount || 0;
  if(item.unit === "grams"){
    const mult = a / 100;
    return mk(item.per100.p*mult, item.per100.c*mult, item.per100.f*mult, item.per100.k*mult);
  }
  // piece / tbsp / cup / count
  const per1 = item.per1 || item.per100; // fallback
  return mk(per1.p*a, per1.c*a, per1.f*a, per1.k*a);
}

function computeCategory(containerId, categoryKey){
  let total = mk(0,0,0,0);
  const rows = el(containerId).querySelectorAll(".dynRow");
  rows.forEach(r=>{
    const sel = r.querySelector("select");
    const amt = r.querySelector('input[type="number"]');
    if(!sel || !amt) return;
    const item = itemByKey(categoryKey, sel.value);
    const amount = parseFloat(amt.value) || 0;
    if(!item || amount<=0) return;
    total = sumMacro(total, macroFromItem(item, amount));
  });
  return total;
}

function computeExercise(){
  let out = {kcal:0, mins:0};
  const rows = el("exerciseRows").querySelectorAll(".dynRow");
  rows.forEach(r=>{
    const inputs = r.querySelectorAll("input");
    const mins = parseFloat(inputs[1]?.value) || 0;
    const kcal = parseFloat(inputs[2]?.value) || 0;
    out.mins += mins;
    out.kcal += kcal;
  });
  return out;
}

/* =========================
   Sleep/Water recommendations
========================= */
function sleepRecommendationHours(age){
  // Based on CDC categories (simplified). :contentReference[oaicite:12]{index=12}
  if(!age || age<=0) return {min:0, max:0, text:"Set age in profile to get sleep guidance."};
  if(age>=18 && age<=60) return {min:7, max:99, text:"Adults (18–60): 7+ hours/night recommended (CDC)."};
  if(age>=61 && age<=64) return {min:7, max:9, text:"Adults (61–64): 7–9 hours/night (CDC)."};
  if(age>=65) return {min:7, max:8, text:"Adults (65+): 7–8 hours/night (CDC)."};
  if(age>=13 && age<=17) return {min:8, max:10, text:"Teens (13–17): 8–10 hours/night (CDC)."};
  if(age>=6 && age<=12) return {min:9, max:12, text:"Kids (6–12): 9–12 hours/night (CDC)."};
  return {min:0, max:0, text:"Sleep guidance: see CDC by age group."};
}

function waterRecommendationLiters(gender){
  // Adequate Intake total water: ~3.7 L men, ~2.7 L women (National Academies). :contentReference[oaicite:13]{index=13}
  // Mayo & Harvard cite similar general numbers. :contentReference[oaicite:14]{index=14}
  if(gender === "female") return {target:2.7, text:"General adequate intake: ~2.7 L/day (women). Adjust for heat/exercise."};
  return {target:3.7, text:"General adequate intake: ~3.7 L/day (men). Adjust for heat/exercise."};
}

/* =========================
   Charts (create once)
========================= */
let macroChart;
function initCharts(){
  const ctx = el("macroChart").getContext("2d");
  macroChart = new Chart(ctx,{
    type:"bar",
    data:{
      labels:["Protein","Fat","Carbs"],
      datasets:[{
        label:"grams",
        data:[0,0,0],
        backgroundColor:[getComputedStyle(document.documentElement).getPropertyValue('--p').trim(),
                         getComputedStyle(document.documentElement).getPropertyValue('--f').trim(),
                         getComputedStyle(document.documentElement).getPropertyValue('--c').trim()]
      }]
    },
    options:{
      responsive:true,
      maintainAspectRatio:false,
      animation:false,
      plugins:{ legend:{display:false}},
      scales:{ y:{ beginAtZero:true } }
    }
  });
}
function updateMacroChart(p,f,c){
  if(!macroChart) return;
  macroChart.data.datasets[0].data = [p,f,c];
  macroChart.update("none");
}

/* =========================
   Daily data IO
========================= */
function extractTodayState(){
  const dateStr = todayKey();

  function rowsToArray(containerId){
    const rows = el(containerId).querySelectorAll(".dynRow");
    const out = [];
    rows.forEach(r=>{
      const sel = r.querySelector("select");
      const nums = r.querySelectorAll('input');
      const amt = parseFloat(nums[0]?.value) || 0;
      const note = nums[1]?.value || "";
      if(sel && (amt>0 || note.trim().length>0)){
        out.push({key: sel.value, amount: amt, note});
      }
    });
    return out;
  }

  function exerciseToArray(){
    const rows = el("exerciseRows").querySelectorAll(".dynRow");
    const out = [];
    rows.forEach(r=>{
      const ins = r.querySelectorAll("input");
      const type = ins[0]?.value || "";
      const mins = parseFloat(ins[1]?.value) || 0;
      const kcal = parseFloat(ins[2]?.value) || 0;
      if(type.trim() || mins>0 || kcal>0) out.push({type, mins, kcal});
    });
    return out;
  }

  return {
    date: dateStr,
    foods:{
      protein: rowsToArray("proteinRows"),
      carbs: rowsToArray("carbRows"),
      fats: rowsToArray("fatRows"),
      snacks: rowsToArray("snackRows"),
      meals: rowsToArray("mealRows")
    },
    lifestyle:{
      sleepHours: numFromInput(el("sleepHours")),
      waterLiters: numFromInput(el("waterLiters")),
      exercise: exerciseToArray()
    }
  };
}

function applyTodayState(state){
  if(!state) return;

  function setRows(containerId, categoryKey, rows){
    el(containerId).innerHTML = "";
    (rows || []).forEach(r=>{
      addFoodRow(containerId, categoryKey, r.key);
      const last = el(containerId).lastElementChild;
      const amt = last.querySelector('input[type="number"]');
      const note = last.querySelectorAll("input")[1];
      if(amt) amt.value = r.amount ?? "";
      if(note) note.value = r.note ?? "";
    });
    if((rows||[]).length === 0){
      // add one default row
      addFoodRow(containerId, categoryKey);
    }
  }

  setRows("proteinRows","protein", state.foods?.protein);
  setRows("carbRows","carbs", state.foods?.carbs);
  setRows("fatRows","fats", state.foods?.fats);
  setRows("snackRows","snacks", state.foods?.snacks);
  setRows("mealRows","meals", state.foods?.meals);

  el("sleepHours").value = state.lifestyle?.sleepHours ?? "";
  el("waterLiters").value = state.lifestyle?.waterLiters ?? "";

  el("exerciseRows").innerHTML = "";
  (state.lifestyle?.exercise || []).forEach(ex=>{
    addExerciseRow();
    const last = el("exerciseRows").lastElementChild;
    const ins = last.querySelectorAll("input");
    ins[0].value = ex.type ?? "";
    ins[1].value = ex.mins ?? "";
    ins[2].value = ex.kcal ?? "";
  });
  if((state.lifestyle?.exercise || []).length === 0){
    addExerciseRow();
  }
}

/* =========================
   Recalc all / dashboard update
========================= */
let calcTimer = null;
function scheduleAll(){
  if(calcTimer) clearTimeout(calcTimer);
  calcTimer = setTimeout(recalcAll, 90);
}

function recalcAll(){
  // profile metrics
  const p = loadProfile();
  const hasProfile = !!p;

  // Update pills
  el("profilePill").textContent = hasProfile ? `Profile: ${p.name || "Saved"}` : "Profile: Not set";
  const goalText = (!hasProfile) ? "Weight loss" : (p.goal==="gain" ? "Weight gain" : p.goal==="maint" ? "Maintenance" : "Weight loss");
  el("goalPill").textContent = goalText;

  // Totals from food
  const P = computeCategory("proteinRows","protein");
  const C = computeCategory("carbRows","carbs");
  const F = computeCategory("fatRows","fats");
  const S = computeCategory("snackRows","snacks");
  const M = computeCategory("mealRows","meals");
  const foodTotal = [P,C,F,S,M].reduce((a,b)=>sumMacro(a,b), mk(0,0,0,0));

  // Exercise
  const ex = computeExercise();
  const kcalIn = foodTotal.k;
  const kcalOut = ex.kcal;
  const kcalNet = kcalIn - kcalOut;

  // Targets
  let tdee = 0, target = 0, protTarget = 0;
  if(hasProfile){
    tdee = calcTDEE(p);
    target = targetCaloriesFromGoal(tdee, p.goal);
    protTarget = p.target_protein || suggestedTargetProtein(p);
  }

  // Dashboard numbers
  el("kcalIn").textContent = round0(kcalIn);
  el("kcalOut").textContent = round0(kcalOut);
  el("kcalNet").textContent = round0(kcalNet);
  el("kcalTarget").textContent = round0(target);

  const kcalBalance = target ? (target - kcalNet) : 0;
  el("kcalBalance").textContent = round0(kcalBalance);

  const protBal = protTarget ? (protTarget - foodTotal.p) : 0;
  el("protBalance").textContent = round0(protBal);

  // Colorize balance boxes
  el("kcalBalanceBox").style.color = (kcalBalance >= 0) ? "var(--good)" : "var(--bad)";
  el("protBalanceBox").style.color = (protBal >= 0) ? "var(--bad)" : "var(--good)";

  // Update chart
  updateMacroChart(parseFloat(foodTotal.p.toFixed(2)), parseFloat(foodTotal.f.toFixed(2)), parseFloat(foodTotal.c.toFixed(2)));

  // Category previews
  setPreview("pv", P);
  setPreview("cv", C);
  setPreview("fv", F);
  setPreview("sv", S);
  setPreview("mv", M);

  // Lifestyle summary + recommendations
  updateLifestyleSummary(p, ex, foodTotal, kcalNet, target);

  // Autosave today
  const tstate = extractTodayState();
  saveDaily(todayKey(), tstate);
}

function setPreview(prefix, m){
  el(prefix+"P").textContent = round2(m.p);
  el(prefix+"F").textContent = round2(m.f);
  el(prefix+"C").textContent = round2(m.c);
  el(prefix+"K").textContent = round0(m.k);
}

function updateLifestyleSummary(p, ex, foodTotal, kcalNet, kcalTarget){
  const sleep = numFromInput(el("sleepHours"));
  const water = numFromInput(el("waterLiters"));

  let lines = [];
  if(!p){
    lines.push("Save your profile to get personalized sleep/water guidance and accurate BMR/TDEE targets.");
    el("lifestyleSummary").innerHTML = lines.join("<br/>");
    el("sleepWaterRec").innerHTML = `Save profile (age + gender) to see sleep/water goals. Sleep: CDC :contentReference[oaicite:15]{index=15} • Water: National Academies/Mayo/Harvard :contentReference[oaicite:16]{index=16}`;
    return;
  }

  // Sleep rec
  const srec = sleepRecommendationHours(p.age);
  const sOk = (srec.min>0) ? (sleep >= srec.min && (srec.max>=90 || sleep <= srec.max)) : false;

  // Water rec
  const wrec = waterRecommendationLiters(p.gender);
  const wOk = water >= wrec.target;

  // Activity guidance note (weekly target)
  const exNote = `Today: ${round0(ex.mins)} min logged, ${round0(ex.kcal)} kcal burned. Weekly target: ≥150 min moderate (WHO/AHA/CDC). :contentReference[oaicite:17]{index=17}`;

  // Goals summary
  const goalName = (p.goal==="gain" ? "Weight gain" : p.goal==="maint" ? "Maintenance" : "Weight loss");
  lines.push(`<b>${goalName}</b> • Net calories today: <b>${round0(kcalNet)}</b> • Target: <b>${round0(kcalTarget)}</b>`);
  lines.push(`<b>Sleep:</b> ${sleep || 0}h • ${srec.text} :contentReference[oaicite:18]{index=18} ${sOk ? "✅" : "⚠️"}`);
  lines.push(`<b>Water:</b> ${water || 0}L • ${wrec.text} :contentReference[oaicite:19]{index=19} ${wOk ? "✅" : "⚠️"}`);
  lines.push(`<b>Exercise:</b> ${exNote}`);

  el("lifestyleSummary").innerHTML = lines.join("<br/>");

  el("sleepWaterRec").innerHTML =
    `<b>Sleep goal:</b> ${srec.min ? (srec.max>=90 ? `${srec.min}+ hours` : `${srec.min}–${srec.max} hours`) : "-"} (CDC). :contentReference[oaicite:20]{index=20}<br/>
     <b>Water goal:</b> ~${wrec.target} L/day (general adequate intake; adjust for heat/exercise). :contentReference[oaicite:21]{index=21}`;
}

/* =========================
   Profile UI wiring
========================= */
function syncHeightWeightUI(){
  const hu = el("p_h_unit").value;
  el("h_cm_row").classList.toggle("hidden", hu !== "cm");
  el("h_ftin_row").classList.toggle("hidden", hu !== "ftin");

  const wu = el("p_w_unit").value;
  el("w_kg_row").classList.toggle("hidden", wu !== "kg");
  el("w_lbs_row").classList.toggle("hidden", wu !== "lbs");
}

function loadProfileToUI(){
  const p = loadProfile();
  if(!p) return;

  el("p_name").value = p.name || "";
  el("p_gender").value = p.gender || "male";
  el("p_age").value = p.age || "";
  el("p_goal").value = p.goal || "loss";
  el("p_h_unit").value = p.h_unit || "cm";
  el("p_w_unit").value = p.w_unit || "kg";
  el("p_activity").value = p.activity || "1.2";

  el("p_h_cm").value = p.h_cm || "";
  el("p_h_ft").value = p.h_ft || "";
  el("p_h_in").value = p.h_in || "";
  el("p_w_kg").value = p.w_kg || "";
  el("p_w_lbs").value = p.w_lbs || "";

  el("p_target_protein").value = p.target_protein || "";

  syncHeightWeightUI();
  updateProfileMetricsCard();
}

function updateProfileMetricsCard(){
  const p = loadProfile();
  if(!p){
    el("m_profileStatus").textContent = "Not saved";
    return;
  }

  const bmi = calcBMI(p);
  const bmr = calcBMR(p);
  const tdee = calcTDEE(p);
  const target = targetCaloriesFromGoal(tdee, p.goal);

  const pr = proteinRange(p);
  const rangeStr = pr.min ? `${Math.round(pr.min)}–${Math.round(pr.max)} g` : "0";

  el("m_bmi").textContent = bmi ? bmi.toFixed(2) : "0";
  el("m_bmr").textContent = bmr ? round0(bmr) : "0";
  el("m_tdee").textContent = tdee ? round0(tdee) : "0";
  el("m_target").textContent = target ? round0(target) : "0";
  el("m_protRange").textContent = rangeStr;
  el("m_profileStatus").textContent = "Saved";

  // BMI marker
  const marker = el("bmiMarker");
  const clip = Math.max(0, Math.min(40, bmi || 0));
  marker.style.left = `${(clip/40)*100}%`;

  // BMI message
  let status = "Save profile to see guidance.";
  if(bmi>0){
    if(bmi < 18.5) status = "Underweight: calorie surplus + protein + strength training can help (if medically appropriate).";
    else if(bmi < 25) status = "Normal range: maintain or lean-gain depending on your goal.";
    else if(bmi < 30) status = "Overweight: fat-loss goal can use a moderate deficit + high protein.";
    else status = "Obese range: prioritize health-first plan and consider clinical guidance.";
  }
  el("bmiMsg").textContent = status;
}

/* =========================
   Page navigation
========================= */
function setPage(page){
  document.querySelectorAll(".tab").forEach(t=>{
    t.classList.toggle("active", t.dataset.page === page);
  });
  el("page-home").classList.toggle("hidden", page !== "home");
  el("page-profile").classList.toggle("hidden", page !== "profile");
  el("page-food").classList.toggle("hidden", page !== "food");
}

/* =========================
   PDF report (colored + charts + summary)
========================= */
async function exportPDF(){
  const p = loadProfile();
  const dateStr = todayKey();
  const day = loadDaily(dateStr) || extractTodayState();

  const P = computeCategory("proteinRows","protein");
  const C = computeCategory("carbRows","carbs");
  const F = computeCategory("fatRows","fats");
  const S = computeCategory("snackRows","snacks");
  const M = computeCategory("mealRows","meals");
  const total = [P,C,F,S,M].reduce((a,b)=>sumMacro(a,b), mk(0,0,0,0));
  const ex = computeExercise();

  const tdee = p ? calcTDEE(p) : 0;
  const target = p ? targetCaloriesFromGoal(tdee, p.goal) : 0;
  const protTarget = p ? (p.target_protein || suggestedTargetProtein(p)) : 0;

  const { jsPDF } = window.jspdf;
  const doc = new jsPDF();

  // Title block
  doc.setFont("helvetica","bold");
  doc.setFontSize(18);
  doc.setTextColor(7,23,52);
  doc.text("Nowshad's Macro Calculator — Daily Report", 10, 14);

  doc.setFont("helvetica","normal");
  doc.setFontSize(11);
  doc.setTextColor(70,80,100);
  doc.text(`Date: ${dateStr}`, 10, 22);

  // Profile summary
  doc.setFont("helvetica","bold");
  doc.setFontSize(13);
  doc.setTextColor(7,23,52);
  doc.text("Profile Summary", 10, 32);

  doc.setFont("helvetica","normal");
  doc.setFontSize(11);
  doc.setTextColor(20,30,45);

  const bmi = p ? calcBMI(p) : 0;
  const bmr = p ? calcBMR(p) : 0;

  const lines = [
    `Name: ${p?.name || "-"}`,
    `Age: ${p?.age || "-"} • Gender: ${p?.gender || "-"}`,
    `BMI: ${bmi ? bmi.toFixed(2) : "-"} • BMR: ${bmr ? round0(bmr) : "-"} kcal • TDEE: ${tdee ? round0(tdee) : "-"} kcal`,
    `Goal: ${p?.goal || "-"} • Target Calories: ${target ? round0(target) : "-"} kcal`,
    `Target Protein: ${protTarget ? round0(protTarget) : "-"} g`
  ];
  doc.text(lines, 10, 40);

  // Macro totals block with color labels
  doc.setFont("helvetica","bold");
  doc.setFontSize(13);
  doc.setTextColor(7,23,52);
  doc.text("Nutrition Summary (Today)", 10, 72);

  doc.setFont("helvetica","bold"); doc.setFontSize(11);
  doc.setTextColor(6,214,160); doc.text(`Protein: ${round2(total.p)} g`, 10, 82);
  doc.setTextColor(239,71,111); doc.text(`Fat: ${round2(total.f)} g`, 10, 90);
  doc.setTextColor(255,209,102); doc.text(`Carbs: ${round2(total.c)} g`, 10, 98);

  doc.setFont("helvetica","bold");
  doc.setTextColor(7,23,52);
  doc.text(`Calories eaten: ${round0(total.k)} kcal`, 10, 110);
  doc.text(`Exercise burned: ${round0(ex.kcal)} kcal`, 10, 118);
  const net = total.k - ex.kcal;
  doc.text(`Net calories: ${round0(net)} kcal`, 10, 126);
  if(target){
    doc.text(`Calories balance (Target - Net): ${round0(target - net)} kcal`, 10, 134);
  }
  if(protTarget){
    doc.text(`Protein balance (Target - Intake): ${round0(protTarget - total.p)} g`, 10, 142);
  }

  // Add chart image
  const canvas = el("macroChart");
  const img = canvas.toDataURL("image/png", 1.0);
  doc.addImage(img, "PNG", 110, 78, 90, 60);

  // Lifestyle block
  doc.setFont("helvetica","bold");
  doc.setFontSize(13);
  doc.setTextColor(7,23,52);
  doc.text("Lifestyle (Today)", 10, 160);

  doc.setFont("helvetica","normal");
  doc.setFontSize(11);
  doc.setTextColor(20,30,45);

  const sleep = numFromInput(el("sleepHours"));
  const water = numFromInput(el("waterLiters"));
  doc.text(`Sleep: ${sleep || 0} hours`, 10, 170);
  doc.text(`Water: ${water || 0} liters`, 10, 178);
  doc.text(`Exercise: ${round0(ex.mins)} minutes • ${round0(ex.kcal)} kcal burned`, 10, 186);

  doc.setFontSize(10);
  doc.setTextColor(90,100,120);
  doc.text("Notes: Sleep guidance is age-based (CDC). Water intake is general AI guidance (National Academies/Mayo/Harvard). Activity guidance references WHO/AHA/CDC weekly targets.", 10, 198, {maxWidth: 190});

  // Disclaimer
  doc.setFontSize(10);
  doc.setTextColor(120,130,150);
  doc.text("Disclaimer: Estimates only. For medical conditions or individualized nutrition planning, consult a clinician or registered dietitian.", 10, 286, {maxWidth: 190});

  doc.save(`Nowshad_Report_${dateStr}.pdf`);
}

/* =========================
   Init / wiring
========================= */
function init(){
  // Date
  el("todayDate").value = todayKey();

  // Charts
  initCharts();

  // Nav
  document.querySelectorAll(".tab").forEach(btn=>{
    btn.addEventListener("click", ()=> setPage(btn.dataset.page));
  });

  // Profile UI toggles
  el("p_h_unit").addEventListener("change", ()=>{ syncHeightWeightUI(); });
  el("p_w_unit").addEventListener("change", ()=>{ syncHeightWeightUI(); });

  // Save profile
  el("btnSaveProfile").addEventListener("click", ()=>{
    const p = {
      name: el("p_name").value.trim(),
      gender: el("p_gender").value,
      age: parseInt(el("p_age").value || "0", 10),
      goal: el("p_goal").value, // default already loss
      h_unit: el("p_h_unit").value,
      w_unit: el("p_w_unit").value,
      activity: el("p_activity").value,

      h_cm: numFromInput(el("p_h_cm")),
      h_ft: numFromInput(el("p_h_ft")),
      h_in: numFromInput(el("p_h_in")),
      w_kg: numFromInput(el("p_w_kg")),
      w_lbs: numFromInput(el("p_w_lbs")),

      target_protein: numFromInput(el("p_target_protein"))
    };

    // Auto-set target protein if empty or 0
    if(!p.target_protein){
      p.target_protein = suggestedTargetProtein(p);
      el("p_target_protein").value = p.target_protein || "";
    }

    saveProfile(p);
    updateProfileMetricsCard();
    scheduleAll();
    alert("Profile saved.");
  });

  el("btnRecalcProfile").addEventListener("click", ()=>{
    const p = loadProfile();
    if(!p){ alert("Save profile first."); return; }
    // update target protein suggestion if field blank
    if(!p.target_protein){
      p.target_protein = suggestedTargetProtein(p);
      saveProfile(p);
      el("p_target_protein").value = p.target_protein || "";
    }
    updateProfileMetricsCard();
    scheduleAll();
  });

  // Add default rows
  addFoodRow("proteinRows","protein","chicken_breast");
  addFoodRow("carbRows","carbs","rice_cooked");
  addFoodRow("fatRows","fats","olive_oil");
  addFoodRow("snackRows","snacks","peanut_butter");
  addFoodRow("mealRows","meals","biryani");
  addExerciseRow();

  // Buttons to add rows
  el("addProteinRow").addEventListener("click", ()=>{ addFoodRow("proteinRows","protein"); scheduleAll(); });
  el("addCarbRow").addEventListener("click", ()=>{ addFoodRow("carbRows","carbs"); scheduleAll(); });
  el("addFatRow").addEventListener("click", ()=>{ addFoodRow("fatRows","fats"); scheduleAll(); });
  el("addSnackRow").addEventListener("click", ()=>{ addFoodRow("snackRows","snacks"); scheduleAll(); });
  el("addMealRow").addEventListener("click", ()=>{ addFoodRow("mealRows","meals"); scheduleAll(); });
  el("addExerciseRow").addEventListener("click", ()=>{ addExerciseRow(); scheduleAll(); });

  // Save today
  el("btnSaveToday").addEventListener("click", ()=>{
    saveDaily(todayKey(), extractTodayState());
    scheduleAll();
    alert("Saved for today.");
  });

  el("btnRecalcNow").addEventListener("click", ()=> scheduleAll());

  // Dashboard buttons
  el("btnPDF").addEventListener("click", exportPDF);
  el("btnClearToday").addEventListener("click", ()=>{
    if(confirm("Clear only today's food + exercise + lifestyle log?")){
      clearDaily(todayKey());
      applyTodayState(null);
      // reset rows
      el("proteinRows").innerHTML=""; el("carbRows").innerHTML=""; el("fatRows").innerHTML="";
      el("snackRows").innerHTML=""; el("mealRows").innerHTML=""; el("exerciseRows").innerHTML="";
      addFoodRow("proteinRows","protein","chicken_breast");
      addFoodRow("carbRows","carbs","rice_cooked");
      addFoodRow("fatRows","fats","olive_oil");
      addFoodRow("snackRows","snacks","peanut_butter");
      addFoodRow("mealRows","meals","biryani");
      addExerciseRow();
      el("sleepHours").value="";
      el("waterLiters").value="";
      scheduleAll();
    }
  });

  el("btnResetAll").addEventListener("click", ()=>{
    if(confirm("This will erase profile + all saved days. Continue?")){
      hardResetAll();
      location.reload();
    }
  });

  // Auto recalc on any input change
  document.addEventListener("input", (e)=>{
    if(e.target && (e.target.tagName==="INPUT" || e.target.tagName==="SELECT" || e.target.tagName==="TEXTAREA")){
      scheduleAll();
    }
  });

  // Load profile + today
  loadProfileToUI();
  updateProfileMetricsCard();

  const savedToday = loadDaily(todayKey());
  if(savedToday){
    applyTodayState(savedToday);
  }

  // Default goal should be weight loss if new profile
  const p = loadProfile();
  if(!p){
    el("p_goal").value = "loss";
  }

  // Initial calc
  scheduleAll();
}

init();
</script>
</body>
</html>
