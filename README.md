
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Nowshad's Macro Calculator</title>

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <style>
    :root{
      --bg1:#062a66;
      --bg2:#0b4db0;
      --card:#ffffff;
      --text:#0b1220;
      --muted:#5b667a;
      --accent:#ffd166;
      --accent2:#06d6a0;
      --danger:#ef476f;
      --line: rgba(15, 23, 42, .12);
      --shadow: 0 18px 50px rgba(0,0,0,.18);
      --radius: 16px;
    }
    *{ box-sizing:border-box; }
    body{
      font-family: "Poppins", system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      margin:0;
      min-height:100vh;
      color:#eaf2ff;
      background:
        radial-gradient(1200px 700px at 20% 10%, rgba(255,209,102,.20), transparent 55%),
        radial-gradient(1000px 600px at 85% 15%, rgba(6,214,160,.18), transparent 55%),
        linear-gradient(135deg, var(--bg1), var(--bg2));
      padding: 24px;
    }
    .container{
      max-width: 1060px;
      margin: 0 auto;
      background: rgba(255,255,255,.97);
      color: var(--text);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .header{
      padding: 22px 22px 16px 22px;
      background: linear-gradient(135deg, rgba(255,209,102,.22), rgba(6,214,160,.14));
      border-bottom: 1px solid var(--line);
    }
    h1{
      margin:0;
      font-size: 22px;
      letter-spacing: .2px;
      color: #0a1a33;
    }
    .sub{
      margin:8px 0 0 0;
      color: var(--muted);
      font-size: 13px;
      line-height: 1.45;
    }

    .grid{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap: 14px;
      padding: 18px 22px 22px 22px;
    }
    @media (max-width: 920px){
      .grid{ grid-template-columns: 1fr; }
    }

    fieldset{
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 14px;
      background: #fff;
    }
    legend{
      padding: 0 10px;
      font-weight: 700;
      font-size: 13px;
      color: #0a1a33;
    }

    .row{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-top: 10px;
    }
    .row.one{ grid-template-columns: 1fr; }

    label{
      display:block;
      font-size: 12px;
      color: var(--muted);
      margin-bottom: 6px;
      font-weight: 600;
    }

    input, select, button{
      width: 100%;
      padding: 10px 12px;
      border: 1px solid rgba(15,23,42,.16);
      border-radius: 12px;
      font-size: 14px;
      outline: none;
      background: #fff;
      color: var(--text);
      transition: box-shadow .15s ease, border-color .15s ease, transform .05s ease;
    }
    input:focus, select:focus{
      border-color: rgba(11,77,176,.45);
      box-shadow: 0 0 0 4px rgba(11,77,176,.12);
    }

    .pill{
      display:flex;
      gap:10px;
      align-items:center;
      justify-content:space-between;
      padding: 12px 14px;
      background: rgba(11,77,176,.06);
      border: 1px solid rgba(11,77,176,.14);
      border-radius: 14px;
      margin-top: 10px;
    }
    .pill strong{ font-size: 12px; color:#0a1a33; }
    .pill span{ font-size: 12px; color: var(--muted); }

    .preview{
      margin-top: 10px;
      padding: 12px 14px;
      border-radius: 14px;
      border: 1px dashed rgba(15,23,42,.18);
      background: rgba(255,209,102,.08);
      color:#0a1a33;
      font-size: 12px;
      line-height: 1.55;
    }

    .muted{ color: var(--muted); }

    .results{
      padding: 0 22px 22px 22px;
      display:grid;
      grid-template-columns: 1fr;
      gap: 12px;
    }
    .kpis{
      display:grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 10px;
    }
    @media (max-width: 980px){ .kpis{ grid-template-columns: repeat(2, 1fr); } }
    .kpi{
      background: #fff;
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 12px 12px;
    }
    .kpi .t{ font-size: 11px; color: var(--muted); font-weight: 600; }
    .kpi .v{ font-size: 18px; font-weight: 800; margin-top: 6px; color:#0a1a33; }

    .danger{ color: var(--danger) !important; }
    .good{ color: #0a7a55 !important; }

    .chartWrap{
      background:#fff;
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 14px;
    }

    .actions{
      padding: 0 22px 22px 22px;
      display:flex;
      gap: 10px;
      flex-wrap: wrap;
    }
    .btn{
      border:none;
      cursor:pointer;
      font-weight: 800;
      letter-spacing: .2px;
      padding: 11px 14px;
    }
    .btn.primary{
      background: linear-gradient(135deg, #0b4db0, #07367f);
      color:#fff;
    }
    .btn.secondary{
      background: #ffffff;
      border: 1px solid rgba(15,23,42,.16);
      color:#0a1a33;
    }
    .btn:active{ transform: translateY(1px); }

    .footer{
      padding: 14px 22px 18px 22px;
      border-top: 1px solid var(--line);
      background: rgba(11,77,176,.04);
      color: var(--muted);
      font-size: 12px;
      line-height: 1.45;
    }
  </style>
</head>

<body>
  <div class="container">
    <div class="header">
      <h1>Nowshad's Macro Calculator</h1>
      <p class="sub">
        1) Enter your details (height/weight/age). 2) Fill food amounts. Macros update automatically. <br/>
        3) Use <b>Goal</b> to estimate target calories for maintenance / deficit / weight gain. <br/>
        You can still press <b>Calculate Macros</b> at the bottom anytime.
      </p>
    </div>

    <div class="grid">
      <!-- USER DETAILS -->
      <fieldset>
        <legend>User details</legend>

        <div class="row">
          <div>
            <label for="name">Name</label>
            <input type="text" id="name" placeholder="Enter name" />
          </div>
          <div>
            <label for="age">Age</label>
            <input type="number" id="age" placeholder="e.g., 30" min="1"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="height_unit">Height unit</label>
            <select id="height_unit">
              <option value="cm">Centimeters (cm)</option>
              <option value="ft_in">Feet/Inches (ft/in)</option>
            </select>
          </div>
          <div>
            <label for="weight_unit">Weight unit</label>
            <select id="weight_unit">
              <option value="kg">Kilograms (kg)</option>
              <option value="lbs">Pounds (lbs)</option>
            </select>
          </div>
        </div>

        <div id="height_cm_input" class="row">
          <div class="one" style="grid-column:1 / -1;">
            <label for="height_cm">Height (cm)</label>
            <input type="number" id="height_cm" placeholder="e.g., 170" min="1"/>
          </div>
        </div>

        <div id="height_ft_in_input" class="row" style="display:none;">
          <div>
            <label for="height_ft">Height (ft)</label>
            <input type="number" id="height_ft" placeholder="Feet" min="0"/>
          </div>
          <div>
            <label for="height_in">Height (in)</label>
            <input type="number" id="height_in" placeholder="Inches" min="0"/>
          </div>
        </div>

        <div id="weight_kg_input" class="row">
          <div class="one" style="grid-column:1 / -1;">
            <label for="weight_kg">Weight (kg)</label>
            <input type="number" id="weight_kg" placeholder="e.g., 70" min="1"/>
          </div>
        </div>

        <div id="weight_lbs_input" class="row" style="display:none;">
          <div class="one" style="grid-column:1 / -1;">
            <label for="weight_lbs">Weight (lbs)</label>
            <input type="number" id="weight_lbs" placeholder="e.g., 154" min="1"/>
          </div>
        </div>

        <div class="pill">
          <div>
            <strong>BMI</strong>
            <span class="muted">(based on height & weight)</span>
          </div>
          <div><b id="bmi">0</b></div>
        </div>

        <div class="pill">
          <div>
            <strong>Recommended protein</strong>
            <span class="muted">(1.5–2.0 g/kg)</span>
          </div>
          <div><b id="recommended_protein">0</b></div>
        </div>

        <div class="row">
          <div class="one" style="grid-column:1 / -1;">
            <label for="target_protein">Target protein (g) — editable</label>
            <input type="number" id="target_protein" value="120" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="activity_level">Activity level</label>
            <select id="activity_level">
              <option value="1.2">Sedentary</option>
              <option value="1.375">Lightly active</option>
              <option value="1.55">Moderately active</option>
              <option value="1.725">Very active</option>
              <option value="1.9">Extra active</option>
            </select>
          </div>
          <div>
            <label for="goal">Goal</label>
            <select id="goal">
              <option value="maintenance">Maintenance</option>
              <option value="deficit">Fat loss (deficit)</option>
              <option value="gain">Weight gain (surplus)</option>
            </select>
          </div>
        </div>

        <div class="preview" id="tdeePreview">
          <b>TDEE:</b> <span id="tdee">0</span> kcal &nbsp;•&nbsp;
          <b>Target calories:</b> <span id="targetCalories">0</span> kcal
          <div class="muted" style="margin-top:6px;">
            Deficit = -500 kcal/day. Weight gain = +400 kcal/day.
          </div>
        </div>
      </fieldset>

      <!-- CHICKEN (FULL SECTION) -->
      <fieldset>
        <legend>Chicken</legend>

        <div class="row">
          <div>
            <label for="chicken_breast">Chicken Breast (grams)</label>
            <input type="number" id="chicken_breast" placeholder="Enter grams" min="0"/>
          </div>
          <div>
            <label for="chicken_thigh">Chicken Thigh (grams)</label>
            <input type="number" id="chicken_thigh" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="chicken_wings">Chicken Wings (grams)</label>
            <input type="number" id="chicken_wings" placeholder="Enter grams" min="0"/>
          </div>
          <div>
            <label for="chicken_drumsticks">Chicken Drumsticks (grams)</label>
            <input type="number" id="chicken_drumsticks" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="ch_protein">0</span> g •
          Fat <span id="ch_fat">0</span> g •
          Carbs <span id="ch_carbs">0</span> g •
          Calories <span id="ch_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- OTHER PROTEIN SOURCES (ALL ORIGINAL ITEMS KEPT) -->
      <fieldset>
        <legend>Other protein sources</legend>

        <div class="row">
          <div>
            <label for="protein_shake">Protein Shake (scoops)</label>
            <input type="number" id="protein_shake" placeholder="Enter scoops" min="0"/>
          </div>
          <div>
            <label for="egg">Eggs (count)</label>
            <input type="number" id="egg" placeholder="Enter count" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="mutton">Mutton (grams)</label>
            <input type="number" id="mutton" placeholder="Enter grams" min="0"/>
          </div>
          <div>
            <label for="duck">Duck (grams)</label>
            <input type="number" id="duck" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="pigeon">Pigeon (grams)</label>
            <input type="number" id="pigeon" placeholder="Enter grams" min="0"/>
          </div>
          <div>
            <label for="quail">Quail (grams)</label>
            <input type="number" id="quail" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="beef">Beef (grams)</label>
            <input type="number" id="beef" placeholder="Enter grams" min="0"/>
          </div>
          <div>
            <label for="fish">Fish (grams)</label>
            <input type="number" id="fish" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="shrimp">Shrimp (grams)</label>
            <input type="number" id="shrimp" placeholder="Enter grams" min="0"/>
          </div>
          <div>
            <label for="dried_fish">Dried Fish / Dried Bombay Duck (grams)</label>
            <input type="number" id="dried_fish" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div class="one" style="grid-column:1 / -1;">
            <label for="yogurt">Yogurt / Curd (grams)</label>
            <input type="number" id="yogurt" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="op_protein">0</span> g •
          Fat <span id="op_fat">0</span> g •
          Carbs <span id="op_carbs">0</span> g •
          Calories <span id="op_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- CARBS + FRUITS + ADDITIONS (KEPT ORIGINAL + NEW) -->
      <fieldset>
        <legend>Carbs, fruits & basics</legend>

        <div class="row">
          <div>
            <label for="rice">Rice (cups)</label>
            <input type="number" id="rice" placeholder="Enter cups" min="0"/>
          </div>
          <div>
            <label for="bread">Bread (slices)</label>
            <input type="number" id="bread" placeholder="Enter slices" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="salad">Salad (cups)</label>
            <input type="number" id="salad" placeholder="Enter cups" min="0"/>
          </div>
          <div>
            <label for="vegetables">Vegetables (cups)</label>
            <input type="number" id="vegetables" placeholder="Enter cups" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="dates">Dates (grams)</label>
            <input type="number" id="dates" placeholder="Enter grams" min="0"/>
          </div>
          <div>
            <label for="peanut_butter">Peanut Butter (tbsp)</label>
            <input type="number" id="peanut_butter" placeholder="Enter tbsp" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="fruit_type">Fruit (select)</label>
            <select id="fruit_type">
              <option value="banana">Banana</option>
              <option value="apple">Apple</option>
              <option value="mango">Mango</option>
              <option value="grapes">Grapes</option>
              <option value="orange">Orange</option>
              <option value="papaya">Papaya</option>
              <option value="watermelon">Watermelon</option>
            </select>
          </div>
          <div>
            <label for="fruit_grams">Fruit amount (grams)</label>
            <input type="number" id="fruit_grams" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="cb_protein">0</span> g •
          Fat <span id="cb_fat">0</span> g •
          Carbs <span id="cb_carbs">0</span> g •
          Calories <span id="cb_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- SNACKS & DRINKS (ORIGINAL ITEMS KEPT) -->
      <fieldset>
        <legend>Snacks & drinks</legend>

        <div class="row">
          <div>
            <label for="milk_tea">Milk Tea (cups)</label>
            <input type="number" id="milk_tea" placeholder="Enter cups" min="0"/>
          </div>
          <div>
            <label for="chocolate">Cube of Chocolate (count)</label>
            <input type="number" id="chocolate" placeholder="Enter count" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="cake">Slice of Cake (count)</label>
            <input type="number" id="cake" placeholder="Enter count" min="0"/>
          </div>
          <div>
            <label for="carbonated_beverage">Carbonated Beverage (cans)</label>
            <input type="number" id="carbonated_beverage" placeholder="Enter cans" min="0"/>
          </div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="sd_protein">0</span> g •
          Fat <span id="sd_fat">0</span> g •
          Carbs <span id="sd_carbs">0</span> g •
          Calories <span id="sd_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- FATS + OILS + NUTS (butter inside fats section; NOT separate) -->
      <fieldset>
        <legend>Fats, oils & nuts</legend>

        <div class="row">
          <div>
            <label for="oil_type">Oil / fat type</label>
            <select id="oil_type">
              <option value="olive_oil">Olive oil (tbsp)</option>
              <option value="soybean_oil">Soybean oil (tbsp)</option>
              <option value="butter">Butter (tbsp)</option>
              <option value="ghee">Ghee (tbsp)</option>
            </select>
          </div>
          <div>
            <label for="oil_tbsp">Oil / fat amount (tbsp)</label>
            <input type="number" id="oil_tbsp" placeholder="Enter tbsp" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="nuts_type">Nuts (select)</label>
            <select id="nuts_type">
              <option value="almonds">Almonds</option>
              <option value="peanuts">Peanuts</option>
              <option value="cashews">Cashew nuts</option>
              <option value="pistachios">Pistachios</option>
              <option value="walnuts">Walnuts</option>
              <option value="hazelnuts">Hazelnuts</option>
            </select>
          </div>
          <div>
            <label for="nuts_grams">Nuts amount (grams)</label>
            <input type="number" id="nuts_grams" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="fn_protein">0</span> g •
          Fat <span id="fn_fat">0</span> g •
          Carbs <span id="fn_carbs">0</span> g •
          Calories <span id="fn_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- MEALS (NEW) -->
      <fieldset>
        <legend>Meals (approx per cup)</legend>

        <div class="row">
          <div>
            <label for="meal_biryani">Chicken Biryani (cups)</label>
            <input type="number" id="meal_biryani" placeholder="Enter cups" min="0"/>
          </div>
          <div>
            <label for="meal_khichuri">Khichuri / Khichdi (cups)</label>
            <input type="number" id="meal_khichuri" placeholder="Enter cups" min="0"/>
          </div>
        </div>

        <div class="row">
          <div class="one" style="grid-column:1 / -1;">
            <label for="meal_fried_rice">Fried Rice (cups)</label>
            <input type="number" id="meal_fried_rice" placeholder="Enter cups" min="0"/>
          </div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="ml_protein">0</span> g •
          Fat <span id="ml_fat">0</span> g •
          Carbs <span id="ml_carbs">0</span> g •
          Calories <span id="ml_cals">0</span> kcal
          <div class="muted" style="margin-top:6px;">
            Meals vary by recipe. These are generic per-cup estimates.
          </div>
        </div>
      </fieldset>
    </div>

    <!-- RESULTS -->
    <div class="results">
      <div class="kpis">
        <div class="kpi">
          <div class="t">Total Protein</div>
          <div class="v"><span id="totalProtein">0</span> g</div>
        </div>
        <div class="kpi">
          <div class="t">Total Fat</div>
          <div class="v"><span id="totalFat">0</span> g</div>
        </div>
        <div class="kpi">
          <div class="t">Total Carbs</div>
          <div class="v"><span id="totalCarbs">0</span> g</div>
        </div>
        <div class="kpi">
          <div class="t">Total Calories</div>
          <div class="v"><span id="totalCalories">0</span> kcal</div>
        </div>
        <div class="kpi">
          <div class="t">Protein balance</div>
          <div class="v" id="proteinBalanceBox"><span id="proteinBalance">0</span> g</div>
        </div>
      </div>

      <div class="chartWrap">
        <div style="display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap;">
          <b style="color:#0a1a33;">Macro Breakdown</b>
          <span class="muted" style="font-size:12px;">Protein / Fat / Carbs grams</span>
        </div>
        <canvas id="macroChart" style="margin-top:12px;"></canvas>
      </div>
    </div>

    <!-- ACTIONS -->
    <div class="actions">
      <button class="btn primary" id="btnCalculate">Calculate Macros</button>
      <button class="btn secondary" id="btnSave">Save Report as PDF</button>
      <button class="btn secondary" id="btnReset" type="button">Reset</button>
    </div>

    <div class="footer">
      <b>Disclaimer:</b> Estimates only. Meals vary by recipe. For personalized dietary advice, consult a certified nutritionist/doctor.
      <br/><br/>
      <b>Key references used for newly added items:</b>
      Butter 14g (100 kcal) , Ghee 13g (112 kcal) , Chicken biryani per cup , Khichdi per cup , Fried rice per cup .
      (All based on common nutrition databases; values can vary by brand/recipe.)
    </div>
  </div>

  <script>
    // -----------------------------
    // Helpers
    // -----------------------------
    function num(id){
      const v = parseFloat(document.getElementById(id)?.value);
      return isNaN(v) ? 0 : v;
    }
    function clampNonNegative(x){ return x < 0 ? 0 : x; }
    function round2(x){ return (Math.round(x * 100) / 100).toFixed(2); }
    function add(a,b){ return { p:a.p+b.p, c:a.c+b.c, f:a.f+b.f, kcal:a.kcal+b.kcal }; }

    // -----------------------------
    // Macro datasets
    // - Some items are "original-style approximations" (same style you used)
    // - Some are from common nutrition databases (per cup/tbsp/per 100g)
    // -----------------------------

    // Oils/Fats per TBSP (kcal from common nutrition databases)
    const fatsPerTbsp = {
      olive_oil:   { p:0,    c:0,    f:13.5,  kcal:119 },
      soybean_oil: { p:0,    c:0,    f:13.6,  kcal:120 },
      butter:      { p:0.12, c:0.01, f:11.36, kcal:100 },
      ghee:        { p:0.04, c:0,    f:12.7,  kcal:112 }
    };

    // Nuts per 100g (common nutrition databases; approximate)
    const nutsPer100g = {
      almonds:    { p:21.2,  c:21.6,  f:49.9,  kcal:579 },
      peanuts:    { p:25.8,  c:16.1,  f:49.2,  kcal:567 },
      cashews:    { p:18.22, c:30.19, f:43.85, kcal:553 },
      pistachios: { p:20.16, c:27.17, f:45.32, kcal:560 },
      walnuts:    { p:15.23, c:13.71, f:65.21, kcal:654 },
      hazelnuts:  { p:14.95, c:16.70, f:60.75, kcal:628 }
    };

    // Fruits per 100g (common nutrition databases; approximate)
    const fruitsPer100g = {
      banana:     { p:1.09, c:22.84, f:0.33, kcal:89 },
      apple:      { p:0.26, c:13.80, f:0.17, kcal:52 },
      mango:      { p:0.51, c:17.00, f:0.27, kcal:65 },
      grapes:     { p:0.72, c:18.10, f:0.16, kcal:69 },
      orange:     { p:0.94, c:11.75, f:0.12, kcal:47 },
      papaya:     { p:0.47, c:10.82, f:0.26, kcal:43 },
      watermelon: { p:0.61, c:7.55,  f:0.15, kcal:30 }
    };

    // Dates per 100g (approx)
    const datesPer100g = { p:2.45, c:75.03, f:0.39, kcal:282 };

    // Yogurt/curd per 100g (approx)
    const yogurtPer100g = { p:3.47, c:4.66, f:3.25, kcal:61 };

    // Peanut butter per tbsp (16g) (approx)
    const peanutButterPerTbsp = { p:3.9, c:2.7, f:8.7, kcal:97 };

    // Original-style approximations (kept for compatibility with your initial calculator)
    const simple = {
      egg:            { p:6,   c:0.6, f:5   },   // per egg (approx)
      protein_shake:  { p:25,  c:3,   f:2   },   // per scoop (typical)
      rice_cup:       { p:4,   c:45,  f:0.4 },   // per cup (rough)
      bread_slice:    { p:3,   c:15,  f:1.2 },   // per slice (rough)
      salad_cup:      { p:0.5, c:2,   f:0.1 },   // per cup (rough)
      veg_cup:        { p:2,   c:5,   f:0.2 },   // per cup (rough)

      // Original “snacks & drinks” from your initial calculator (kept)
      milk_tea_cup:   { p:0,   c:10,  f:0   },   // per cup (as you had)
      chocolate_cube: { p:1,   c:15,  f:5   },   // per cube (as you had)
      cake_slice:     { p:3,   c:30,  f:10  },   // per slice (as you had)
      soda_can:       { p:0,   c:40,  f:0   }    // per can (as you had)
    };

    // Meat/protein multipliers (original approach — per gram)
    // Protein multipliers copied from your initial calculator; fats kept aligned with your original fat section.
    const meats = {
      // Chicken (protein per 100g -> per gram multiplier)
      chicken_breast:     { pPerGram: 0.31, fPerGram: 0.00, cPerGram: 0 },
      chicken_thigh:      { pPerGram: 0.26, fPerGram: 0.08, cPerGram: 0 },
      chicken_wings:      { pPerGram: 0.30, fPerGram: 0.00, cPerGram: 0 },
      chicken_drumsticks: { pPerGram: 0.28, fPerGram: 0.10, cPerGram: 0 },

      // Other protein sources (protein per 100g -> per gram)
      mutton:             { pPerGram: 0.25, fPerGram: 0.20, cPerGram: 0 },
      duck:               { pPerGram: 0.23, fPerGram: 0.28, cPerGram: 0 },
      pigeon:             { pPerGram: 0.25, fPerGram: 0.15, cPerGram: 0 },
      quail:              { pPerGram: 0.22, fPerGram: 0.12, cPerGram: 0 },
      beef:               { pPerGram: 0.26, fPerGram: 0.15, cPerGram: 0 },
      fish:               { pPerGram: 0.22, fPerGram: 0.05, cPerGram: 0 },
      shrimp:             { pPerGram: 0.24, fPerGram: 0.02, cPerGram: 0 },
      dried_fish:         { pPerGram: 0.60, fPerGram: 0.05, cPerGram: 0 }
    };

    // Meals per CUP (generic database estimates)
    const mealsPerCup = {
      chicken_biryani: { p:15.90, c:48.07, f:9.82,  kcal:348 },
      khichdi:         { p:6.12,  c:34.28, f:1.78,  kcal:175 },
      fried_rice:      { p:12.47, c:41.70, f:12.34, kcal:333 }
    };

    // -----------------------------
    // UI toggle helpers
    // -----------------------------
    function toggleHeightInput(){
      const heightUnit = document.getElementById('height_unit').value;
      document.getElementById('height_cm_input').style.display = (heightUnit === 'cm') ? 'grid' : 'none';
      document.getElementById('height_ft_in_input').style.display = (heightUnit === 'ft_in') ? 'grid' : 'none';
    }
    function toggleWeightInput(){
      const weightUnit = document.getElementById('weight_unit').value;
      document.getElementById('weight_kg_input').style.display = (weightUnit === 'kg') ? 'grid' : 'none';
      document.getElementById('weight_lbs_input').style.display = (weightUnit === 'lbs') ? 'grid' : 'none';
    }

    function getHeightMeters(){
      const heightUnit = document.getElementById('height_unit').value;
      if (heightUnit === 'cm') return num('height_cm') / 100;
      const ft = num('height_ft'), inch = num('height_in');
      return (ft * 0.3048) + (inch * 0.0254);
    }
    function getWeightKg(){
      const weightUnit = document.getElementById('weight_unit').value;
      const w = num(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs');
      return (weightUnit === 'lbs') ? (w * 0.453592) : w;
    }

    function calcBMI(){
      const h = getHeightMeters(), w = getWeightKg();
      if (h <= 0 || w <= 0) return 0;
      return w / (h*h);
    }
    function calcRecommendedProtein(){
      const w = getWeightKg();
      if (w <= 0) return "0";
      const min = (w * 1.5).toFixed(0);
      const max = (w * 2.0).toFixed(0);
      return `${min}–${max} g`;
    }

    function calcTDEEAndTarget(){
      const w = getWeightKg();
      const h = getHeightMeters();
      const age = num('age');
      const activity = parseFloat(document.getElementById('activity_level').value);
      const goal = document.getElementById('goal').value;

      if (w <= 0 || h <= 0 || age <= 0) return { tdee:0, target:0 };

      // Mifflin-St Jeor (male default, same as your original)
      const bmr = (10 * w) + (6.25 * h * 100) - (5 * age) + 5;
      const tdee = bmr * activity;

      let target = tdee;
      if (goal === 'deficit') target = tdee - 500;
      if (goal === 'gain')    target = tdee + 400;

      return { tdee, target };
    }

    // -----------------------------
    // Section computations
    // -----------------------------
    function computeChickenSection(){
      const breast = num('chicken_breast');
      const thigh  = num('chicken_thigh');
      const wings  = num('chicken_wings');
      const drum   = num('chicken_drumsticks');

      const p = (breast * meats.chicken_breast.pPerGram) +
                (thigh  * meats.chicken_thigh.pPerGram) +
                (wings  * meats.chicken_wings.pPerGram) +
                (drum   * meats.chicken_drumsticks.pPerGram);

      const f = (breast * meats.chicken_breast.fPerGram) +
                (thigh  * meats.chicken_thigh.fPerGram) +
                (wings  * meats.chicken_wings.fPerGram) +
                (drum   * meats.chicken_drumsticks.fPerGram);

      const c = 0;
      const kcal = (p*4) + (c*4) + (f*9);
      return {p,c,f,kcal};
    }

    function computeOtherProteinSection(){
      let total = {p:0,c:0,f:0,kcal:0};

      // shakes + eggs
      const scoops = num('protein_shake');
      total = add(total, {
        p: scoops * simple.protein_shake.p,
        c: scoops * simple.protein_shake.c,
        f: scoops * simple.protein_shake.f,
        kcal: (scoops * simple.protein_shake.p * 4) + (scoops * simple.protein_shake.c * 4) + (scoops * simple.protein_shake.f * 9)
      });

      const eggs = num('egg');
      total = add(total, {
        p: eggs * simple.egg.p,
        c: eggs * simple.egg.c,
        f: eggs * simple.egg.f,
        kcal: (eggs * simple.egg.p * 4) + (eggs * simple.egg.c * 4) + (eggs * simple.egg.f * 9)
      });

      // meats (grams)
      const ids = ["mutton","duck","pigeon","quail","beef","fish","shrimp","dried_fish"];
      ids.forEach(id=>{
        const g = num(id);
        const m = meats[id];
        const p = g * m.pPerGram;
        const f = g * m.fPerGram;
        const c = g * m.cPerGram;
        total = add(total, {p,c,f,kcal:(p*4)+(c*4)+(f*9)});
      });

      // yogurt/curd grams (per 100g)
      const yg = num('yogurt');
      total = add(total, {
        p: (yg/100) * yogurtPer100g.p,
        c: (yg/100) * yogurtPer100g.c,
        f: (yg/100) * yogurtPer100g.f,
        kcal: (yg/100) * yogurtPer100g.kcal
      });

      return total;
    }

    function computeCarbsFruitsBasics(){
      let total = {p:0,c:0,f:0,kcal:0};

      const rice = num('rice');
      total = add(total, {
        p: rice * simple.rice_cup.p,
        c: rice * simple.rice_cup.c,
        f: rice * simple.rice_cup.f,
        kcal: (rice * simple.rice_cup.p * 4) + (rice * simple.rice_cup.c * 4) + (rice * simple.rice_cup.f * 9)
      });

      const bread = num('bread');
      total = add(total, {
        p: bread * simple.bread_slice.p,
        c: bread * simple.bread_slice.c,
        f: bread * simple.bread_slice.f,
        kcal: (bread * simple.bread_slice.p * 4) + (bread * simple.bread_slice.c * 4) + (bread * simple.bread_slice.f * 9)
      });

      const salad = num('salad');
      total = add(total, {
        p: salad * simple.salad_cup.p,
        c: salad * simple.salad_cup.c,
        f: salad * simple.salad_cup.f,
        kcal: (salad * simple.salad_cup.p * 4) + (salad * simple.salad_cup.c * 4) + (salad * simple.salad_cup.f * 9)
      });

      const veg = num('vegetables');
      total = add(total, {
        p: veg * simple.veg_cup.p,
        c: veg * simple.veg_cup.c,
        f: veg * simple.veg_cup.f,
        kcal: (veg * simple.veg_cup.p * 4) + (veg * simple.veg_cup.c * 4) + (veg * simple.veg_cup.f * 9)
      });

      // dates (g)
      const d = num('dates');
      total = add(total, {
        p: (d/100) * datesPer100g.p,
        c: (d/100) * datesPer100g.c,
        f: (d/100) * datesPer100g.f,
        kcal: (d/100) * datesPer100g.kcal
      });

      // peanut butter (tbsp)
      const pb = num('peanut_butter');
      total = add(total, {
        p: pb * peanutButterPerTbsp.p,
        c: pb * peanutButterPerTbsp.c,
        f: pb * peanutButterPerTbsp.f,
        kcal: pb * peanutButterPerTbsp.kcal
      });

      // fruit dropdown (g)
      const fruitType = document.getElementById('fruit_type').value;
      const fg = num('fruit_grams');
      const fruit = fruitsPer100g[fruitType] || {p:0,c:0,f:0,kcal:0};
      total = add(total, {
        p: (fg/100) * fruit.p,
        c: (fg/100) * fruit.c,
        f: (fg/100) * fruit.f,
        kcal: (fg/100) * fruit.kcal
      });

      return total;
    }

    function computeSnacksDrinks(){
      let total = {p:0,c:0,f:0,kcal:0};

      const mt = num('milk_tea');
      total = add(total, {
        p: mt * simple.milk_tea_cup.p,
        c: mt * simple.milk_tea_cup.c,
        f: mt * simple.milk_tea_cup.f,
        kcal: (mt * simple.milk_tea_cup.p * 4) + (mt * simple.milk_tea_cup.c * 4) + (mt * simple.milk_tea_cup.f * 9)
      });

      const ch = num('chocolate');
      total = add(total, {
        p: ch * simple.chocolate_cube.p,
        c: ch * simple.chocolate_cube.c,
        f: ch * simple.chocolate_cube.f,
        kcal: (ch * simple.chocolate_cube.p * 4) + (ch * simple.chocolate_cube.c * 4) + (ch * simple.chocolate_cube.f * 9)
      });

      const ck = num('cake');
      total = add(total, {
        p: ck * simple.cake_slice.p,
        c: ck * simple.cake_slice.c,
        f: ck * simple.cake_slice.f,
        kcal: (ck * simple.cake_slice.p * 4) + (ck * simple.cake_slice.c * 4) + (ck * simple.cake_slice.f * 9)
      });

      const soda = num('carbonated_beverage');
      total = add(total, {
        p: soda * simple.soda_can.p,
        c: soda * simple.soda_can.c,
        f: soda * simple.soda_can.f,
        kcal: (soda * simple.soda_can.p * 4) + (soda * simple.soda_can.c * 4) + (soda * simple.soda_can.f * 9)
      });

      return total;
    }

    function computeFatsNuts(){
      let total = {p:0,c:0,f:0,kcal:0};

      // oil/fat tbsp
      const oilType = document.getElementById('oil_type').value;
      const tbsp = num('oil_tbsp');
      const o = fatsPerTbsp[oilType] || {p:0,c:0,f:0,kcal:0};
      total = add(total, { p:tbsp*o.p, c:tbsp*o.c, f:tbsp*o.f, kcal:tbsp*o.kcal });

      // nuts grams
      const nt = document.getElementById('nuts_type').value;
      const ng = num('nuts_grams');
      const n = nutsPer100g[nt] || {p:0,c:0,f:0,kcal:0};
      total = add(total, {
        p: (ng/100) * n.p,
        c: (ng/100) * n.c,
        f: (ng/100) * n.f,
        kcal: (ng/100) * n.kcal
      });

      return total;
    }

    function computeMeals(){
      let total = {p:0,c:0,f:0,kcal:0};
      const b = num('meal_biryani');
      const k = num('meal_khichuri');
      const fr = num('meal_fried_rice');

      total = add(total, {
        p: b * mealsPerCup.chicken_biryani.p,
        c: b * mealsPerCup.chicken_biryani.c,
        f: b * mealsPerCup.chicken_biryani.f,
        kcal: b * mealsPerCup.chicken_biryani.kcal
      });

      total = add(total, {
        p: k * mealsPerCup.khichdi.p,
        c: k * mealsPerCup.khichdi.c,
        f: k * mealsPerCup.khichdi.f,
        kcal: k * mealsPerCup.khichdi.kcal
      });

      total = add(total, {
        p: fr * mealsPerCup.fried_rice.p,
        c: fr * mealsPerCup.fried_rice.c,
        f: fr * mealsPerCup.fried_rice.f,
        kcal: fr * mealsPerCup.fried_rice.kcal
      });

      return total;
    }

    function setPreview(prefix, m){
      document.getElementById(prefix + '_protein').innerText = round2(m.p);
      document.getElementById(prefix + '_fat').innerText     = round2(m.f);
      document.getElementById(prefix + '_carbs').innerText   = round2(m.c);
      document.getElementById(prefix + '_cals').innerText    = round2(m.kcal);
    }

    // -----------------------------
    // Chart
    // -----------------------------
    let macroChart;
    function updateChart(protein, fat, carbs){
      const ctx = document.getElementById('macroChart').getContext('2d');
      if (macroChart) macroChart.destroy();
      macroChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ['Protein', 'Fat', 'Carbohydrates'],
          datasets: [{
            label: 'Macro Breakdown (g)',
            data: [protein, fat, carbs],
            // 3 different colors, none blue
            backgroundColor: ['#06D6A0', '#EF476F', '#FFD166']
          }]
        },
        options: {
          responsive: true,
          scales: { y: { beginAtZero: true } },
          plugins: { legend: { display: false } }
        }
      });
    }

    // -----------------------------
    // Calculate all (auto)
    // -----------------------------
    function calculateAll(){
      toggleHeightInput();
      toggleWeightInput();

      const bmi = calcBMI();
      document.getElementById('bmi').innerText = bmi ? bmi.toFixed(2) : "0";
      document.getElementById('recommended_protein').innerText = calcRecommendedProtein();

      const { tdee, target } = calcTDEEAndTarget();
      document.getElementById('tdee').innerText = tdee.toFixed(0);
      document.getElementById('targetCalories').innerText = target.toFixed(0);

      // Sections
      const chicken = computeChickenSection();
      const otherP  = computeOtherProteinSection();
      const carbsB  = computeCarbsFruitsBasics();
      const snacks  = computeSnacksDrinks();
      const fatsN   = computeFatsNuts();
      const meals   = computeMeals();

      // Section previews
      // Chicken
      document.getElementById('ch_protein').innerText = round2(chicken.p);
      document.getElementById('ch_fat').innerText     = round2(chicken.f);
      document.getElementById('ch_carbs').innerText   = round2(chicken.c);
      document.getElementById('ch_cals').innerText    = round2(chicken.kcal);

      // Other protein
      document.getElementById('op_protein').innerText = round2(otherP.p);
      document.getElementById('op_fat').innerText     = round2(otherP.f);
      document.getElementById('op_carbs').innerText   = round2(otherP.c);
      document.getElementById('op_cals').innerText    = round2(otherP.kcal);

      // Carbs/basics
      document.getElementById('cb_protein').innerText = round2(carbsB.p);
      document.getElementById('cb_fat').innerText     = round2(carbsB.f);
      document.getElementById('cb_carbs').innerText   = round2(carbsB.c);
      document.getElementById('cb_cals').innerText    = round2(carbsB.kcal);

      // Snacks/drinks
      document.getElementById('sd_protein').innerText = round2(snacks.p);
      document.getElementById('sd_fat').innerText     = round2(snacks.f);
      document.getElementById('sd_carbs').innerText   = round2(snacks.c);
      document.getElementById('sd_cals').innerText    = round2(snacks.kcal);

      // Fats/nuts
      document.getElementById('fn_protein').innerText = round2(fatsN.p);
      document.getElementById('fn_fat').innerText     = round2(fatsN.f);
      document.getElementById('fn_carbs').innerText   = round2(fatsN.c);
      document.getElementById('fn_cals').innerText    = round2(fatsN.kcal);

      // Meals
      document.getElementById('ml_protein').innerText = round2(meals.p);
      document.getElementById('ml_fat').innerText     = round2(meals.f);
      document.getElementById('ml_carbs').innerText   = round2(meals.c);
      document.getElementById('ml_cals').innerText    = round2(meals.kcal);

      // Totals
      const total = add(add(add(add(add(chicken, otherP), carbsB), snacks), fatsN), meals);

      document.getElementById('totalProtein').innerText = round2(total.p);
      document.getElementById('totalFat').innerText     = round2(total.f);
      document.getElementById('totalCarbs').innerText   = round2(total.c);

      // Calories: use summed kcal (meals/oils/dates etc already have kcal); for “approx blocks” also computed.
      document.getElementById('totalCalories').innerText = round2(total.kcal);

      // Protein balance
      const targetProtein = clampNonNegative(num('target_protein'));
      const balance = targetProtein - total.p;
      document.getElementById('proteinBalance').innerText = round2(balance);

      const box = document.getElementById('proteinBalanceBox');
      box.classList.remove('danger','good');
      if (balance > 0.01) box.classList.add('danger'); else box.classList.add('good');

      // Chart
      updateChart(total.p, total.f, total.c);
    }

    // -----------------------------
    // PDF report
    // -----------------------------
    function saveReport(){
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();

      doc.setFontSize(18);
      doc.text("Nowshad's Macro Calculator Report", 10, 12);

      const date = new Date().toLocaleString();
      doc.setFontSize(11);
      doc.text(`Generated: ${date}`, 10, 20);

      doc.setFontSize(13);
      doc.text("User Details", 10, 32);
      doc.setFontSize(11);
      doc.text(`Name: ${document.getElementById('name').value || "-"}`, 10, 40);
      doc.text(`Age: ${document.getElementById('age').value || "-"}`, 10, 47);

      const heightText = (document.getElementById('height_unit').value === 'cm')
        ? `${document.getElementById('height_cm').value || "-"} cm`
        : `${document.getElementById('height_ft').value || "-"} ft ${document.getElementById('height_in').value || "-"} in`;

      const weightText = (document.getElementById('weight_unit').value === 'kg')
        ? `${document.getElementById('weight_kg').value || "-"} kg`
        : `${document.getElementById('weight_lbs').value || "-"} lbs`;

      doc.text(`Height: ${heightText}`, 10, 54);
      doc.text(`Weight: ${weightText}`, 10, 61);
      doc.text(`BMI: ${document.getElementById('bmi').innerText}`, 10, 68);
      doc.text(`Recommended Protein: ${document.getElementById('recommended_protein').innerText}`, 10, 75);

      doc.setFontSize(13);
      doc.text("Totals", 10, 88);
      doc.setFontSize(11);
      doc.text(`Protein: ${document.getElementById('totalProtein').innerText} g`, 10, 96);
      doc.text(`Fat: ${document.getElementById('totalFat').innerText} g`, 10, 103);
      doc.text(`Carbs: ${document.getElementById('totalCarbs').innerText} g`, 10, 110);
      doc.text(`Calories: ${document.getElementById('totalCalories').innerText} kcal`, 10, 117);

      doc.text(`Target Protein: ${document.getElementById('target_protein').value || "0"} g`, 10, 126);
      doc.text(`Protein Balance: ${document.getElementById('proteinBalance').innerText} g`, 10, 133);

      doc.setFontSize(13);
      doc.text("Energy Targets", 10, 146);
      doc.setFontSize(11);
      doc.text(`TDEE: ${document.getElementById('tdee').innerText} kcal`, 10, 154);
      doc.text(`Target Calories: ${document.getElementById('targetCalories').innerText} kcal`, 10, 161);

      doc.setFontSize(9);
      doc.text("Disclaimer: estimates only. Meals vary by recipe. Consult a professional for personalized advice.", 10, 176, { maxWidth: 190 });

      doc.save("Nowshad_Macro_Calculator_Report.pdf");
    }

    // -----------------------------
    // Reset
    // -----------------------------
    function resetAll(){
      const ids = [
        "name","age","height_cm","height_ft","height_in","weight_kg","weight_lbs",
        "target_protein",
        // chicken
        "chicken_breast","chicken_thigh","chicken_wings","chicken_drumsticks",
        // other protein sources
        "protein_shake","egg","mutton","duck","pigeon","quail","beef","fish","shrimp","dried_fish","yogurt",
        // carbs/basics
        "rice","bread","salad","vegetables","dates","peanut_butter","fruit_grams",
        // snacks/drinks
        "milk_tea","chocolate","cake","carbonated_beverage",
        // fats/nuts
        "oil_tbsp","nuts_grams",
        // meals
        "meal_biryani","meal_khichuri","meal_fried_rice"
      ];
      ids.forEach(id => { const el = document.getElementById(id); if(el) el.value = ""; });

      document.getElementById('height_unit').value = "cm";
      document.getElementById('weight_unit').value = "kg";
      document.getElementById('activity_level').value = "1.2";
      document.getElementById('goal').value = "maintenance";
      document.getElementById('fruit_type').value = "banana";
      document.getElementById('oil_type').value = "olive_oil";
      document.getElementById('nuts_type').value = "almonds";
      document.getElementById('target_protein').value = "120";

      calculateAll();
    }

    // -----------------------------
    // Bind auto-calc
    // -----------------------------
    function bindAutoCalc(){
      const all = document.querySelectorAll("input, select");
      all.forEach(el => {
        el.addEventListener("input", calculateAll);
        el.addEventListener("change", calculateAll);
      });

      document.getElementById("height_unit").addEventListener("change", toggleHeightInput);
      document.getElementById("weight_unit").addEventListener("change", toggleWeightInput);

      document.getElementById("btnCalculate").addEventListener("click", (e) => {
        e.preventDefault();
        calculateAll();
      });

      document.getElementById("btnSave").addEventListener("click", (e) => {
        e.preventDefault();
        saveReport();
      });

      document.getElementById("btnReset").addEventListener("click", (e) => {
        e.preventDefault();
        resetAll();
      });
    }

    // Init
    bindAutoCalc();
    calculateAll();
  </script>
</body>
</html>
