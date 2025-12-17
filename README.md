
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
      background: radial-gradient(1200px 700px at 20% 10%, rgba(255,209,102,.20), transparent 55%),
                  radial-gradient(1000px 600px at 85% 15%, rgba(6,214,160,.18), transparent 55%),
                  linear-gradient(135deg, var(--bg1), var(--bg2));
      padding: 24px;
    }

    .container{
      max-width: 980px;
      margin: 0 auto;
      background: rgba(255,255,255,.96);
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
    @media (max-width: 860px){
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
    .preview b{ color:#0a1a33; }
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
        Fill in your details and food amounts. Macros update automatically as you type.
        Use <b>Goal</b> to estimate maintenance, deficit, or weight-gain calories.
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
            <label for="target_protein">Target protein (g) — editable (default set for convenience)</label>
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
            Deficit uses -500 kcal/day. Weight gain uses +400 kcal/day.
          </div>
        </div>
      </fieldset>

      <!-- OTHER PROTEIN SOURCES -->
      <fieldset>
        <legend>Other protein sources</legend>

        <div class="row">
          <div>
            <label for="protein_shake">Protein shake (scoops)</label>
            <input type="number" id="protein_shake" placeholder="e.g., 2" min="0"/>
          </div>
          <div>
            <label for="egg">Eggs (count)</label>
            <input type="number" id="egg" placeholder="e.g., 4" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="chicken_breast">Chicken breast (g)</label>
            <input type="number" id="chicken_breast" placeholder="grams" min="0"/>
          </div>
          <div>
            <label for="beef">Beef (g)</label>
            <input type="number" id="beef" placeholder="grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="fish">Fish (g)</label>
            <input type="number" id="fish" placeholder="grams" min="0"/>
          </div>
          <div>
            <label for="shrimp">Shrimp (g)</label>
            <input type="number" id="shrimp" placeholder="grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="yogurt">Yogurt/curd (g)</label>
            <input type="number" id="yogurt" placeholder="grams" min="0"/>
          </div>
          <div>
            <label for="dried_fish">Dried fish (g)</label>
            <input type="number" id="dried_fish" placeholder="grams" min="0"/>
          </div>
        </div>

        <div class="preview" id="proteinSectionPreview">
          <b>Instant preview:</b>
          Protein <span id="p_protein">0</span> g •
          Fat <span id="p_fat">0</span> g •
          Carbs <span id="p_carbs">0</span> g •
          Calories <span id="p_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- CARBS + FRUITS + CALORIE BOOSTERS -->
      <fieldset>
        <legend>Carbs, fruits & calorie boosters</legend>

        <div class="row">
          <div>
            <label for="rice">Rice (cups)</label>
            <input type="number" id="rice" placeholder="cups" min="0"/>
          </div>
          <div>
            <label for="bread">Bread (slices)</label>
            <input type="number" id="bread" placeholder="slices" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="dates">Dates (g)</label>
            <input type="number" id="dates" placeholder="grams" min="0"/>
          </div>
          <div>
            <label for="peanut_butter">Peanut butter (tbsp)</label>
            <input type="number" id="peanut_butter" placeholder="tbsp" min="0"/>
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
            </select>
          </div>
          <div>
            <label for="fruit_grams">Fruit amount (g)</label>
            <input type="number" id="fruit_grams" placeholder="grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="salad">Salad (cups)</label>
            <input type="number" id="salad" placeholder="cups" min="0"/>
          </div>
          <div>
            <label for="vegetables">Vegetables (cups)</label>
            <input type="number" id="vegetables" placeholder="cups" min="0"/>
          </div>
        </div>

        <div class="preview" id="carbSectionPreview">
          <b>Instant preview:</b>
          Protein <span id="c_protein">0</span> g •
          Fat <span id="c_fat">0</span> g •
          Carbs <span id="c_carbs">0</span> g •
          Calories <span id="c_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- FATS + NUTS -->
      <fieldset>
        <legend>Fats, oils & nuts</legend>

        <div class="row">
          <div>
            <label for="oil_type">Oil / fat type</label>
            <select id="oil_type">
              <option value="olive_oil">Olive oil</option>
              <option value="soybean_oil">Soybean oil</option>
              <option value="butter">Butter</option>
              <option value="ghee">Ghee</option>
            </select>
          </div>
          <div>
            <label for="oil_tbsp">Oil / fat amount (tbsp)</label>
            <input type="number" id="oil_tbsp" placeholder="tbsp" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="nuts_type">Nuts (select)</label>
            <select id="nuts_type">
              <option value="almonds">Almonds</option>
              <option value="peanuts">Peanuts</option>
              <option value="cashews">Cashews</option>
            </select>
          </div>
          <div>
            <label for="nuts_grams">Nuts amount (g)</label>
            <input type="number" id="nuts_grams" placeholder="grams" min="0"/>
          </div>
        </div>

        <div class="preview" id="fatSectionPreview">
          <b>Instant preview:</b>
          Protein <span id="f_protein">0</span> g •
          Fat <span id="f_fat">0</span> g •
          Carbs <span id="f_carbs">0</span> g •
          Calories <span id="f_cals">0</span> kcal
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

    <!-- ACTIONS (Calculate button at bottom as requested) -->
    <div class="actions">
      <button class="btn primary" id="btnCalculate">Calculate Macros</button>
      <button class="btn secondary" id="btnSave">Save Report as PDF</button>
      <button class="btn secondary" id="btnReset" type="button">Reset</button>
    </div>

    <div class="footer">
      <b>Disclaimer:</b> This tool provides estimates. For medical conditions or exact planning, consult a certified nutritionist/doctor.
      <br/><br/>
      <b>Macro data sources used (serving references):</b>
      Butter (1 tbsp/14g) from MyFoodData. Ghee (1 tbsp/13g) from MyFoodData. Olive oil (1 tbsp/13.5g) from MyFoodData. Soybean oil (1 tbsp/13.6g) from MyFoodData.
      Peanut butter (1 tbsp/16g) from MyFoodData. Almonds (per 100g) from MyFoodData. Peanuts (per 100g) from MyFoodData. Cashews (per 100g) from MyFoodData/FatSecret.
      Dates Deglet Noor (per 100g) from FatSecret. Banana (per 100g) from NutritionDataHub/FatSecret. Apple (per 100g) from NutritionDataHub. Mango (per 100g) from FatSecret. Grapes (per 100g) from FatSecret.
      Yogurt whole milk (per 100g) from FatSecret.
    </div>
  </div>

  <script>
    // -----------------------------
    // Macro dataset (normalized)
    // Units:
    //  - per100g items: grams input
    //  - tbsp items: tbsp input
    // Notes: Calories are derived from 4/4/9 unless a direct serving is used (we also include kcal where needed).
    // -----------------------------

    // Oils/Fats per TBSP
    // Olive oil 1 tbsp (13.5g) = 13.5g fat, 0g carbs, 0g protein, 119 kcal (MyFoodData) [turn0search9]
    // Soybean oil 1 tbsp (14g/13.6g shown) ~ 13.6g fat, 0g carbs, 0g protein, 120 kcal (MyFoodData) [turn0search10/turn0search12]
    // Butter 1 tbsp (14g) = 11g fat, 0g carbs, 0g protein, 100 kcal (MyFoodData) [turn0search11]
    // Ghee 1 tbsp (13g) = 12.7g fat, 0g carbs, ~0g protein, 112 kcal (MyFoodData) [turn1search0]
    const fatsPerTbsp = {
      olive_oil:  { p:0,   c:0,   f:13.5, kcal:119 },
      soybean_oil:{ p:0,   c:0,   f:13.6, kcal:120 },
      butter:     { p:0,   c:0,   f:11.0, kcal:100 },
      ghee:       { p:0.04,c:0,   f:12.7, kcal:112 }
    };

    // Nuts per 100g
    // Almonds 100g: P 21.2g, C 21.6g, F 49.9g, 579 kcal (MyFoodData) [turn1search10]
    // Peanuts raw 100g: P 25.8g, C 16.1g, F 49.2g, 567 kcal (MyFoodData) [turn5search0]
    // Cashews 100g: P 18.22g, C 30.19g, F 43.85g, 553 kcal (FatSecret/USDA) [turn6search6]
    const nutsPer100g = {
      almonds: { p:21.2,  c:21.6,  f:49.9,  kcal:579 },
      peanuts: { p:25.8,  c:16.1,  f:49.2,  kcal:567 },
      cashews: { p:18.22, c:30.19, f:43.85, kcal:553 }
    };

    // Fruits per 100g
    // Banana 100g: P 1.09, C 22.84, F 0.33, 89 kcal (FatSecret/USDA) [turn7search4]
    // Apple 100g: P 0.26, C 13.80, F 0.17, 52 kcal (NutritionDataHub) [turn2search10]
    // Mango 100g: P 0.51, C 17.00, F 0.27, 65 kcal (FatSecret/USDA) [turn7search1]
    // Grapes 100g: P 0.72, C 18.1, F 0.16, 69 kcal (FatSecret) [turn7search2]
    const fruitsPer100g = {
      banana: { p:1.09, c:22.84, f:0.33, kcal:89 },
      apple:  { p:0.26, c:13.80, f:0.17, kcal:52 },
      mango:  { p:0.51, c:17.00, f:0.27, kcal:65 },
      grapes: { p:0.72, c:18.10, f:0.16, kcal:69 }
    };

    // Dates per 100g
    // Deglet Noor dates 100g: P 2.45, C 75.03, F 0.39, 282 kcal (FatSecret/USDA) [turn1search11]
    const datesPer100g = { p:2.45, c:75.03, f:0.39, kcal:282 };

    // Yogurt per 100g
    // Whole milk plain yogurt 100g: P 3.47, C 4.66, F 3.25, 61 kcal (FatSecret/USDA) [turn7search3]
    const yogurtPer100g = { p:3.47, c:4.66, f:3.25, kcal:61 };

    // Peanut butter per TBSP
    // 1 tbsp (16g): P 3.9, C 2.7, F 8.7, 97 kcal (MyFoodData) [turn1search5]
    const peanutButterPerTbsp = { p:3.9, c:2.7, f:8.7, kcal:97 };

    // Existing simple items (kept from your version, but harmonized)
    // These are approximations used previously:
    const simple = {
      egg: { p:6, c:0.6, f:5 },          // per egg (approx)
      protein_shake: { p:25, c:3, f:2 }, // per scoop (typical)
      rice_cup: { p:4, c:45, f:0.4 },    // per cup cooked (rough)
      bread_slice: { p:3, c:15, f:1.2 }, // per slice (rough)
      salad_cup: { p:0.5, c:2, f:0.1 },  // per cup (rough)
      veg_cup: { p:2, c:5, f:0.2 }       // per cup (rough)
    };

    // Lean meat/fish approximations you already used (per gram protein multipliers)
    // We keep your approach for these for continuity.
    const meats = {
      chicken_breast: { pPerGram: 0.31, fPerGram: 0.036, cPerGram: 0, kcalPerGram: 1.65 }, // approx per 100g: P31 F3.6 ~165kcal
      beef:           { pPerGram: 0.26, fPerGram: 0.15,  cPerGram: 0, kcalPerGram: 2.5  }, // rough
      fish:           { pPerGram: 0.22, fPerGram: 0.05,  cPerGram: 0, kcalPerGram: 1.2  }, // rough
      shrimp:         { pPerGram: 0.24, fPerGram: 0.02,  cPerGram: 0, kcalPerGram: 0.99 }, // rough
      dried_fish:     { pPerGram: 0.60, fPerGram: 0.05,  cPerGram: 0, kcalPerGram: 2.9  }  // rough
    };

    let macroChart;

    function num(id){
      const v = parseFloat(document.getElementById(id)?.value);
      return isNaN(v) ? 0 : v;
    }

    function clampNonNegative(x){ return x < 0 ? 0 : x; }

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
      if (heightUnit === 'cm'){
        return num('height_cm') / 100;
      }
      const ft = num('height_ft');
      const inch = num('height_in');
      return (ft * 0.3048) + (inch * 0.0254);
    }

    function getWeightKg(){
      const weightUnit = document.getElementById('weight_unit').value;
      const w = num(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs');
      return (weightUnit === 'lbs') ? (w * 0.453592) : w;
    }

    function calcBMI(){
      const h = getHeightMeters();
      const w = getWeightKg();
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

      // Mifflin-St Jeor (male default as in your original)
      const bmr = (10 * w) + (6.25 * h * 100) - (5 * age) + 5;
      const tdee = bmr * activity;

      let target = tdee;
      if (goal === 'deficit') target = tdee - 500;
      if (goal === 'gain')    target = tdee + 400;

      return { tdee, target };
    }

    function addMacros(a,b){
      return {
        p: a.p + b.p,
        c: a.c + b.c,
        f: a.f + b.f,
        kcal: a.kcal + b.kcal
      };
    }

    function round2(x){ return (Math.round(x * 100) / 100).toFixed(2); }

    // Section computations
    function computeProteinSection(){
      let total = {p:0,c:0,f:0,kcal:0};

      const scoops = num('protein_shake');
      total = addMacros(total, {
        p: scoops * simple.protein_shake.p,
        c: scoops * simple.protein_shake.c,
        f: scoops * simple.protein_shake.f,
        kcal: (scoops * simple.protein_shake.p * 4) + (scoops * simple.protein_shake.c * 4) + (scoops * simple.protein_shake.f * 9)
      });

      const eggs = num('egg');
      total = addMacros(total, {
        p: eggs * simple.egg.p,
        c: eggs * simple.egg.c,
        f: eggs * simple.egg.f,
        kcal: (eggs * simple.egg.p * 4) + (eggs * simple.egg.c * 4) + (eggs * simple.egg.f * 9)
      });

      // Meats (grams)
      const cb = num('chicken_breast');
      total = addMacros(total, {
        p: cb * meats.chicken_breast.pPerGram,
        c: cb * meats.chicken_breast.cPerGram,
        f: cb * meats.chicken_breast.fPerGram,
        kcal: cb * meats.chicken_breast.kcalPerGram
      });

      const beef = num('beef');
      total = addMacros(total, {
        p: beef * meats.beef.pPerGram,
        c: beef * meats.beef.cPerGram,
        f: beef * meats.beef.fPerGram,
        kcal: beef * meats.beef.kcalPerGram
      });

      const fish = num('fish');
      total = addMacros(total, {
        p: fish * meats.fish.pPerGram,
        c: fish * meats.fish.cPerGram,
        f: fish * meats.fish.fPerGram,
        kcal: fish * meats.fish.kcalPerGram
      });

      const shrimp = num('shrimp');
      total = addMacros(total, {
        p: shrimp * meats.shrimp.pPerGram,
        c: shrimp * meats.shrimp.cPerGram,
        f: shrimp * meats.shrimp.fPerGram,
        kcal: shrimp * meats.shrimp.kcalPerGram
      });

      const df = num('dried_fish');
      total = addMacros(total, {
        p: df * meats.dried_fish.pPerGram,
        c: df * meats.dried_fish.cPerGram,
        f: df * meats.dried_fish.fPerGram,
        kcal: df * meats.dried_fish.kcalPerGram
      });

      // Yogurt/curd (grams, per 100g dataset)
      const yg = num('yogurt');
      total = addMacros(total, {
        p: (yg/100) * yogurtPer100g.p,
        c: (yg/100) * yogurtPer100g.c,
        f: (yg/100) * yogurtPer100g.f,
        kcal: (yg/100) * yogurtPer100g.kcal
      });

      return total;
    }

    function computeCarbSection(){
      let total = {p:0,c:0,f:0,kcal:0};

      const rice = num('rice');
      total = addMacros(total, {
        p: rice * simple.rice_cup.p,
        c: rice * simple.rice_cup.c,
        f: rice * simple.rice_cup.f,
        kcal: (rice * simple.rice_cup.p * 4) + (rice * simple.rice_cup.c * 4) + (rice * simple.rice_cup.f * 9)
      });

      const bread = num('bread');
      total = addMacros(total, {
        p: bread * simple.bread_slice.p,
        c: bread * simple.bread_slice.c,
        f: bread * simple.bread_slice.f,
        kcal: (bread * simple.bread_slice.p * 4) + (bread * simple.bread_slice.c * 4) + (bread * simple.bread_slice.f * 9)
      });

      // Dates (grams)
      const d = num('dates');
      total = addMacros(total, {
        p: (d/100) * datesPer100g.p,
        c: (d/100) * datesPer100g.c,
        f: (d/100) * datesPer100g.f,
        kcal: (d/100) * datesPer100g.kcal
      });

      // Peanut butter (tbsp)
      const pb = num('peanut_butter');
      total = addMacros(total, {
        p: pb * peanutButterPerTbsp.p,
        c: pb * peanutButterPerTbsp.c,
        f: pb * peanutButterPerTbsp.f,
        kcal: pb * peanutButterPerTbsp.kcal
      });

      // Fruit dropdown (grams)
      const fruitType = document.getElementById('fruit_type').value;
      const fg = num('fruit_grams');
      const fruit = fruitsPer100g[fruitType] || {p:0,c:0,f:0,kcal:0};
      total = addMacros(total, {
        p: (fg/100) * fruit.p,
        c: (fg/100) * fruit.c,
        f: (fg/100) * fruit.f,
        kcal: (fg/100) * fruit.kcal
      });

      const salad = num('salad');
      total = addMacros(total, {
        p: salad * simple.salad_cup.p,
        c: salad * simple.salad_cup.c,
        f: salad * simple.salad_cup.f,
        kcal: (salad * simple.salad_cup.p * 4) + (salad * simple.salad_cup.c * 4) + (salad * simple.salad_cup.f * 9)
      });

      const veg = num('vegetables');
      total = addMacros(total, {
        p: veg * simple.veg_cup.p,
        c: veg * simple.veg_cup.c,
        f: veg * simple.veg_cup.f,
        kcal: (veg * simple.veg_cup.p * 4) + (veg * simple.veg_cup.c * 4) + (veg * simple.veg_cup.f * 9)
      });

      return total;
    }

    function computeFatSection(){
      let total = {p:0,c:0,f:0,kcal:0};

      // Oil/Fat type (tbsp)
      const oilType = document.getElementById('oil_type').value;
      const tbsp = num('oil_tbsp');
      const o = fatsPerTbsp[oilType] || {p:0,c:0,f:0,kcal:0};
      total = addMacros(total, {
        p: tbsp * o.p,
        c: tbsp * o.c,
        f: tbsp * o.f,
        kcal: tbsp * o.kcal
      });

      // Nuts dropdown (grams)
      const nt = document.getElementById('nuts_type').value;
      const ng = num('nuts_grams');
      const n = nutsPer100g[nt] || {p:0,c:0,f:0,kcal:0};
      total = addMacros(total, {
        p: (ng/100) * n.p,
        c: (ng/100) * n.c,
        f: (ng/100) * n.f,
        kcal: (ng/100) * n.kcal
      });

      return total;
    }

    function updateSectionPreview(prefix, macros){
      document.getElementById(prefix + '_protein').innerText = round2(macros.p);
      document.getElementById(prefix + '_fat').innerText     = round2(macros.f);
      document.getElementById(prefix + '_carbs').innerText   = round2(macros.c);
      document.getElementById(prefix + '_cals').innerText    = round2(macros.kcal);
    }

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
            // Must be 3 different colors, none blue
            backgroundColor: ['#06D6A0', '#EF476F', '#FFD166']
          }]
        },
        options: {
          responsive: true,
          scales: {
            y: { beginAtZero: true }
          },
          plugins: {
            legend: { display: false }
          }
        }
      });
    }

    function calculateAll(){
      // Toggle blocks
      toggleHeightInput();
      toggleWeightInput();

      // BMI + recommended protein
      const bmi = calcBMI();
      document.getElementById('bmi').innerText = bmi ? bmi.toFixed(2) : "0";
      document.getElementById('recommended_protein').innerText = calcRecommendedProtein();

      // TDEE + target
      const { tdee, target } = calcTDEEAndTarget();
      document.getElementById('tdee').innerText = tdee.toFixed(0);
      document.getElementById('targetCalories').innerText = target.toFixed(0);

      // Sections
      const secP = computeProteinSection();
      const secC = computeCarbSection();
      const secF = computeFatSection();

      updateSectionPreview('p', secP);
      updateSectionPreview('c', secC);
      updateSectionPreview('f', secF);

      // Totals
      const total = addMacros(addMacros(secP, secC), secF);

      document.getElementById('totalProtein').innerText = round2(total.p);
      document.getElementById('totalFat').innerText     = round2(total.f);
      document.getElementById('totalCarbs').innerText   = round2(total.c);

      // Calories: use dataset kcal sums where present, but fall back to 4/4/9 if needed
      const calories = total.kcal > 0 ? total.kcal : ((total.p*4) + (total.c*4) + (total.f*9));
      document.getElementById('totalCalories').innerText = round2(calories);

      // Protein balance vs target protein
      const targetProtein = clampNonNegative(num('target_protein'));
      const balance = targetProtein - total.p;
      document.getElementById('proteinBalance').innerText = round2(balance);

      const box = document.getElementById('proteinBalanceBox');
      box.classList.remove('danger','good');
      // If balance > 0 => still short, else met/exceeded
      if (balance > 0.01) box.classList.add('danger');
      else box.classList.add('good');

      // Chart
      updateChart(total.p, total.f, total.c);
    }

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
      doc.text("Disclaimer: estimates only. Consult a professional for personalized advice.", 10, 176, { maxWidth: 190 });

      doc.save("Nowshad_Macro_Calculator_Report.pdf");
    }

    function resetAll(){
      const ids = [
        "name","age","height_cm","height_ft","height_in","weight_kg","weight_lbs",
        "protein_shake","egg","chicken_breast","beef","fish","shrimp","yogurt","dried_fish",
        "rice","bread","dates","peanut_butter","fruit_grams","salad","vegetables",
        "oil_tbsp","nuts_grams"
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

    // Auto-calc hooks
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
