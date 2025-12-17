
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Nowshad's Macro Calculator</title>

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&display=swap" rel="stylesheet">

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <style>
    :root{
      --bg1:#062a66;
      --bg2:#0b4db0;
      --text:#0b1220;
      --muted:#5b667a;
      --danger:#ef476f;
      --good:#06d6a0;
      --warn:#ffd166;
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
      padding: 14px;
    }
    .container{
      max-width: 1100px;
      margin: 0 auto;
      background: rgba(255,255,255,.97);
      color: var(--text);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .header{
      padding: 18px 16px 12px 16px;
      background: linear-gradient(135deg, rgba(255,209,102,.22), rgba(6,214,160,.14));
      border-bottom: 1px solid var(--line);
    }
    h1{ margin:0; font-size: 20px; color:#0a1a33; letter-spacing:.2px; }
    .sub{
      margin:8px 0 0 0; color: var(--muted); font-size: 13px; line-height: 1.45;
    }

    .grid{
      display:grid;
      grid-template-columns: 1fr;
      gap: 12px;
      padding: 14px 14px 18px 14px;
    }
    @media (min-width: 920px){
      .grid{ grid-template-columns: 1fr 1fr; gap: 14px; padding: 18px 22px 22px 22px; }
      .header{ padding: 22px 22px 16px 22px; }
      h1{ font-size: 22px; }
      body{ padding: 24px; }
    }

    fieldset{
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 12px;
      background: #fff;
      min-width: 0;
    }
    legend{
      padding: 0 10px;
      font-weight: 800;
      font-size: 13px;
      color: #0a1a33;
    }

    .row{
      display:grid;
      grid-template-columns: 1fr;
      gap: 10px;
      margin-top: 10px;
      min-width: 0;
    }
    @media (min-width: 640px){
      .row{ grid-template-columns: 1fr 1fr; }
    }

    label{
      display:block;
      font-size: 12px;
      color: var(--muted);
      margin-bottom: 6px;
      font-weight: 700;
    }

    input, select, button{
      width: 100%;
      max-width: 100%;
      padding: 10px 12px;
      border: 1px solid rgba(15,23,42,.16);
      border-radius: 12px;
      font-size: 14px;
      outline: none;
      background: #fff;
      color: var(--text);
    }

    .pill{
      display:flex;
      gap:10px;
      align-items:center;
      justify-content:space-between;
      padding: 10px 12px;
      background: rgba(11,77,176,.06);
      border: 1px solid rgba(11,77,176,.14);
      border-radius: 14px;
      margin-top: 10px;
    }
    .pill strong{ font-size: 12px; color:#0a1a33; }
    .pill span{ font-size: 12px; color: var(--muted); }

    .preview{
      margin-top: 10px;
      padding: 10px 12px;
      border-radius: 14px;
      border: 1px dashed rgba(15,23,42,.18);
      background: rgba(255,209,102,.08);
      color:#0a1a33;
      font-size: 12px;
      line-height: 1.55;
      word-break: break-word;
    }

    .miniHelp{
      margin-top: 8px;
      font-size: 12px;
      color: var(--muted);
      line-height: 1.4;
    }

    .results{
      padding: 0 14px 16px 14px;
      display:grid;
      gap: 12px;
    }
    @media (min-width: 920px){
      .results{ padding: 0 22px 22px 22px; }
    }

    .kpis{
      display:grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
    }
    @media (min-width: 920px){
      .kpis{ grid-template-columns: repeat(5, 1fr); }
    }

    .kpi{
      background: #fff;
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 12px 12px;
      min-width: 0;
    }
    .kpi .t{ font-size: 11px; color: var(--muted); font-weight: 700; }
    .kpi .v{ font-size: 18px; font-weight: 900; margin-top: 6px; color:#0a1a33; }

    .danger{ color: var(--danger) !important; }
    .good{ color: #0a7a55 !important; }

    .chartWrap{
      background:#fff;
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 12px;
      min-width: 0;
      position: relative;
    }
    /* ✅ Fix chart jumping / “keeps going down”: stable fixed height */
    .chartBox{
      height: 260px;
      position: relative;
    }
    canvas{ display:block; }

    .actions{
      padding: 0 14px 18px 14px;
      display:flex;
      gap: 10px;
      flex-wrap: wrap;
    }
    @media (min-width: 920px){
      .actions{ padding: 0 22px 22px 22px; }
    }

    .btn{
      border:none;
      cursor:pointer;
      font-weight: 900;
      letter-spacing: .2px;
      padding: 11px 14px;
      border-radius: 12px;
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

    /* ✅ BMI range bar */
    .bmiBar{
      margin-top: 10px;
      height: 14px;
      border-radius: 999px;
      border: 1px solid rgba(15,23,42,.16);
      background:
        linear-gradient(to right,
          #06d6a0 0%, #06d6a0 45%,
          #ffd166 45%, #ffd166 70%,
          #ef476f 70%, #ef476f 100%
        );
      position: relative;
      overflow: hidden;
    }
    .bmiMarker{
      position:absolute;
      top:-6px;
      width: 2px;
      height: 26px;
      background:#0a1a33;
      box-shadow: 0 0 0 2px rgba(255,255,255,.9);
    }
    .bmiLegend{
      display:flex;
      justify-content: space-between;
      margin-top: 6px;
      font-size: 11px;
      color: var(--muted);
      gap: 8px;
      flex-wrap: wrap;
    }
    .bmiMsg{
      margin-top: 8px;
      font-size: 12px;
      color:#0a1a33;
      background: rgba(11,77,176,.06);
      border: 1px solid rgba(11,77,176,.14);
      border-radius: 12px;
      padding: 10px 12px;
      line-height: 1.45;
    }

    /* ✅ Highlight TDEE/Target */
    .tdeeBadge{
      margin-top: 10px;
      padding: 12px 12px;
      border-radius: 14px;
      border: 1px solid rgba(6,214,160,.25);
      background: rgba(6,214,160,.10);
      color:#0a1a33;
    }
    .tdeeBadge .big{
      font-size: 16px;
      font-weight: 900;
    }
    .tdeeBadge .big span{
      font-weight: 900;
      letter-spacing: .2px;
    }
    .tdeeBadge .hint{
      margin-top: 6px;
      font-size: 12px;
      color: var(--muted);
      line-height: 1.4;
    }

    /* dynamic rows (multi-select) */
    .stack{ display:grid; gap: 10px; margin-top: 10px; }
    .dynRow{
      display:grid;
      grid-template-columns: 1.1fr .8fr .8fr auto;
      gap: 8px;
      align-items:end;
    }
    @media (max-width: 640px){
      .dynRow{ grid-template-columns: 1fr 1fr; }
      .dynRow .span2{ grid-column: span 2; }
      .dynRow .btnMini{ grid-column: span 2; }
    }
    .btnMini{
      padding: 10px 12px;
      border-radius: 12px;
      font-weight: 900;
      cursor:pointer;
      border: 1px solid rgba(15,23,42,.16);
      background:#fff;
      color:#0a1a33;
      width: 100%;
    }
    .removeBtn{
      padding: 10px 10px;
      border-radius: 12px;
      border: 1px solid rgba(239,71,111,.35);
      background: rgba(239,71,111,.08);
      color:#0a1a33;
      cursor:pointer;
      font-weight: 900;
      min-width: 44px;
    }
  </style>
</head>

<body>
  <div class="container">
    <div class="header">
      <h1>Nowshad's Macro Calculator</h1>
      <p class="sub">Enter details → fill food inputs. Macros update automatically. Use “Goal” to estimate target calories.</p>
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
          <div>
            <label for="height_cm">Height (cm)</label>
            <input type="number" id="height_cm" placeholder="e.g., 170" min="1"/>
          </div>
          <div></div>
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
          <div>
            <label for="weight_kg">Weight (kg)</label>
            <input type="number" id="weight_kg" placeholder="e.g., 70" min="1"/>
          </div>
          <div></div>
        </div>

        <div id="weight_lbs_input" class="row" style="display:none;">
          <div>
            <label for="weight_lbs">Weight (lbs)</label>
            <input type="number" id="weight_lbs" placeholder="e.g., 154" min="1"/>
          </div>
          <div></div>
        </div>

        <div class="pill">
          <div>
            <strong>BMI</strong>
            <span>(recommended: <b>18.5–24.9</b>)</span>
          </div>
          <div><b id="bmi">0</b></div>
        </div>

        <div class="bmiBar" aria-label="BMI Range Bar">
          <div class="bmiMarker" id="bmiMarker" style="left:0%;"></div>
        </div>
        <div class="bmiLegend">
          <span><b>Under</b> &lt;18.5</span>
          <span><b>Normal</b> 18.5–24.9</span>
          <span><b>Over</b> 25–29.9</span>
          <span><b>Obese</b> ≥30</span>
        </div>
        <div class="bmiMsg" id="bmiMsg">Enter height & weight to see your BMI status and guidance.</div>

        <div class="pill">
          <div>
            <strong>Recommended protein</strong>
            <span>(1.5–2.0 g/kg)</span>
          </div>
          <div><b id="recommended_protein">0</b></div>
        </div>

        <div class="row">
          <div>
            <label for="target_protein">Target protein (g) — auto-set above minimum (editable)</label>
            <input type="number" id="target_protein" value="0" min="0"/>
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

        <div class="row">
          <div>
            <label for="activity_level">Activity level (with workout-days guide)</label>
            <select id="activity_level">
              <option value="1.2">Sedentary</option>
              <option value="1.375">Light</option>
              <option value="1.55">Moderate</option>
              <option value="1.725">Very Active</option>
              <option value="1.9">Extra Active</option>
            </select>
            <div class="miniHelp" id="activityHint">
              Sedentary = little/no exercise • Light = 1–3x/week • Moderate = 4–5x/week • Active/Very Active = 6–7x/week (see note below)
            </div>
            <div class="miniHelp">
              Calculator.net notes: exercise ~15–30 min; intense 45–120 min; very intense 2+ hours. :contentReference[oaicite:2]{index=2}
            </div>
          </div>
          <div>
            <div class="tdeeBadge">
              <div class="big">
                TDEE: <span id="tdee">0</span> kcal • Target: <span id="targetCalories">0</span> kcal
              </div>
              <div class="hint" id="goalHint">Maintenance = same as TDEE • Deficit = -500 kcal/day • Gain = +400 kcal/day</div>
            </div>
          </div>
        </div>
      </fieldset>

      <!-- CHICKEN -->
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

      <!-- OTHER PROTEIN SOURCES -->
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
            <label for="dried_fish">Dried Fish / Bombay Duck (grams)</label>
            <input type="number" id="dried_fish" placeholder="Enter grams" min="0"/>
          </div>
        </div>

        <div class="row">
          <div>
            <label for="yogurt">Yogurt / Curd (grams)</label>
            <input type="number" id="yogurt" placeholder="Enter grams" min="0"/>
          </div>
          <div></div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="op_protein">0</span> g •
          Fat <span id="op_fat">0</span> g •
          Carbs <span id="op_carbs">0</span> g •
          Calories <span id="op_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- CARBS / BASICS / DATES (multi) -->
      <fieldset>
        <legend>Carbs & basics</legend>

        <!-- Rice with plate fractions -->
        <div class="row">
          <div>
            <label>Rice input mode</label>
            <select id="rice_mode">
              <option value="grams">Grams</option>
              <option value="plate">Plate (fraction)</option>
            </select>
          </div>
          <div>
            <label>Rice grams per full plate (editable)</label>
            <input type="number" id="rice_plate_grams" value="250" min="0"/>
          </div>
        </div>
        <div class="row" id="rice_grams_row">
          <div>
            <label for="rice_grams">Rice (grams)</label>
            <input type="number" id="rice_grams" placeholder="Enter grams" min="0"/>
          </div>
          <div></div>
        </div>
        <div class="row" id="rice_plate_row" style="display:none;">
          <div>
            <label for="rice_plate_fraction">Rice plate amount</label>
            <select id="rice_plate_fraction">
              <option value="0">0</option>
              <option value="0.25">1/4 plate</option>
              <option value="0.5">1/2 plate</option>
              <option value="1">1 full plate</option>
              <option value="1.5">1.5 plates</option>
              <option value="2">2 plates</option>
            </select>
          </div>
          <div></div>
        </div>

        <div class="row">
          <div>
            <label for="bread">Bread (slices)</label>
            <input type="number" id="bread" placeholder="Enter slices" min="0"/>
          </div>
          <div>
            <label for="vegetables">Vegetables (cups)</label>
            <input type="number" id="vegetables" placeholder="Enter cups" min="0"/>
          </div>
        </div>

        <!-- Dates: multi rows grams OR pcs with Ajwa/Sukkari -->
        <div class="stack">
          <div style="display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap;">
            <div style="font-weight:900; color:#0a1a33;">Dates (add multiple)</div>
            <button class="btnMini" type="button" id="addDateRow">+ Add dates row</button>
          </div>
          <div id="dateRows"></div>
          <div class="miniHelp">
            Reference examples: “1 date (8g) ≈ 23 kcal” (generic) :contentReference[oaicite:3]{index=3} • Ajwa example: 20g serving ≈ 50 kcal (brand entry) :contentReference[oaicite:4]{index=4} • Sukkari example (per 100g): 268 kcal, carbs 62g, protein 2g, fat 0g :contentReference[oaicite:5]{index=5}
          </div>
        </div>

        <div class="row">
          <div>
            <label for="peanut_butter">Peanut Butter (tbsp)</label>
            <input type="number" id="peanut_butter" placeholder="Enter tbsp" min="0"/>
          </div>
          <div>
            <label for="salad">Salad (cups)</label>
            <input type="number" id="salad" placeholder="Enter cups" min="0"/>
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

      <!-- FRUITS (multi) -->
      <fieldset>
        <legend>Fruits (add multiple)</legend>
        <button class="btnMini" type="button" id="addFruitRow">+ Add fruit</button>
        <div class="miniHelp">
          Size references: medium banana ≈ 118g :contentReference[oaicite:6]{index=6} • medium apple ≈ 182g :contentReference[oaicite:7]{index=7} • orange ≈ 131g :contentReference[oaicite:8]{index=8}
        </div>
        <div id="fruitRows" class="stack"></div>
        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="fr_protein">0</span> g •
          Fat <span id="fr_fat">0</span> g •
          Carbs <span id="fr_carbs">0</span> g •
          Calories <span id="fr_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- FATS/OILS (multi) -->
      <fieldset>
        <legend>Oils & fats (add multiple)</legend>
        <button class="btnMini" type="button" id="addOilRow">+ Add oil/fat</button>
        <div id="oilRows" class="stack"></div>
        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="fn_protein">0</span> g •
          Fat <span id="fn_fat">0</span> g •
          Carbs <span id="fn_carbs">0</span> g •
          Calories <span id="fn_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- NUTS (multi) -->
      <fieldset>
        <legend>Nuts (add multiple)</legend>
        <button class="btnMini" type="button" id="addNutRow">+ Add nuts</button>
        <div id="nutRows" class="stack"></div>
        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="nt_protein">0</span> g •
          Fat <span id="nt_fat">0</span> g •
          Carbs <span id="nt_carbs">0</span> g •
          Calories <span id="nt_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- MEALS -->
      <fieldset>
        <legend>Meals (grams OR plate fractions)</legend>

        <div class="row">
          <div>
            <label>Meal plate grams (editable)</label>
            <input type="number" id="meal_plate_grams" value="350" min="0"/>
          </div>
          <div class="miniHelp">
            Tip: If a “full plate” in your house is bigger/smaller, adjust this once.
          </div>
        </div>

        <div class="row">
          <div>
            <label>Chicken Biryani mode</label>
            <select id="biryani_mode">
              <option value="grams">Grams</option>
              <option value="plate">Plate fraction</option>
            </select>
          </div>
          <div id="biryani_grams_box">
            <label for="meal_biryani_grams">Chicken Biryani (grams)</label>
            <input type="number" id="meal_biryani_grams" min="0" placeholder="Enter grams">
          </div>
          <div id="biryani_plate_box" style="display:none;">
            <label for="meal_biryani_plate">Chicken Biryani (plate)</label>
            <select id="meal_biryani_plate">
              <option value="0">0</option>
              <option value="0.25">1/4 plate</option>
              <option value="0.5">1/2 plate</option>
              <option value="1">1 plate</option>
              <option value="1.5">1.5 plates</option>
              <option value="2">2 plates</option>
            </select>
          </div>
        </div>

        <div class="row">
          <div>
            <label>Khichuri mode</label>
            <select id="khichuri_mode">
              <option value="grams">Grams</option>
              <option value="plate">Plate fraction</option>
            </select>
          </div>
          <div id="khichuri_grams_box">
            <label for="meal_khichuri_grams">Khichuri (grams)</label>
            <input type="number" id="meal_khichuri_grams" min="0" placeholder="Enter grams">
          </div>
          <div id="khichuri_plate_box" style="display:none;">
            <label for="meal_khichuri_plate">Khichuri (plate)</label>
            <select id="meal_khichuri_plate">
              <option value="0">0</option>
              <option value="0.25">1/4 plate</option>
              <option value="0.5">1/2 plate</option>
              <option value="1">1 plate</option>
              <option value="1.5">1.5 plates</option>
              <option value="2">2 plates</option>
            </select>
          </div>
        </div>

        <div class="row">
          <div>
            <label>Fried Rice mode</label>
            <select id="friedrice_mode">
              <option value="grams">Grams</option>
              <option value="plate">Plate fraction</option>
            </select>
          </div>
          <div id="friedrice_grams_box">
            <label for="meal_friedrice_grams">Fried Rice (grams)</label>
            <input type="number" id="meal_friedrice_grams" min="0" placeholder="Enter grams">
          </div>
          <div id="friedrice_plate_box" style="display:none;">
            <label for="meal_friedrice_plate">Fried Rice (plate)</label>
            <select id="meal_friedrice_plate">
              <option value="0">0</option>
              <option value="0.25">1/4 plate</option>
              <option value="0.5">1/2 plate</option>
              <option value="1">1 plate</option>
              <option value="1.5">1.5 plates</option>
              <option value="2">2 plates</option>
            </select>
          </div>
        </div>

        <div class="preview">
          <b>Instant preview:</b>
          Protein <span id="ml_protein">0</span> g •
          Fat <span id="ml_fat">0</span> g •
          Carbs <span id="ml_carbs">0</span> g •
          Calories <span id="ml_cals">0</span> kcal
        </div>
      </fieldset>

      <!-- SNACKS & DRINKS -->
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
          <span class="miniHelp">Stable chart (updates without re-creating)</span>
        </div>
        <div class="chartBox">
          <canvas id="macroChart"></canvas>
        </div>
      </div>
    </div>

    <div class="actions">
      <button class="btn primary" id="btnCalculate">Calculate Macros</button>
      <button class="btn secondary" id="btnSave">Save Report as PDF</button>
      <button class="btn secondary" id="btnReset" type="button">Reset</button>
    </div>

    <div class="footer">
      <b>Disclaimer:</b> Values are estimates. For clinical/medical needs, consult a qualified nutrition professional.
    </div>
  </div>

  <script>
    // ========= Helpers =========
    function num(id){
      const el = document.getElementById(id);
      if(!el) return 0;
      const v = parseFloat(el.value);
      return isNaN(v) ? 0 : v;
    }
    function clampNonNegative(x){ return x < 0 ? 0 : x; }
    function round2(x){ return (Math.round(x * 100) / 100).toFixed(2); }
    function roundUpToNearest10(x){ return Math.ceil(x / 10) * 10; }

    function getWeightKg(){
      const unit = document.getElementById('weight_unit').value;
      const w = parseFloat(document.getElementById(unit === 'kg' ? 'weight_kg' : 'weight_lbs').value);
      if (isNaN(w)) return 0;
      return (unit === 'lbs') ? (w * 0.453592) : w;
    }
    function getHeightMeters(){
      const unit = document.getElementById('height_unit').value;
      if (unit === 'cm'){
        const cm = parseFloat(document.getElementById('height_cm').value);
        return isNaN(cm) ? 0 : cm / 100;
      }
      const ft = parseFloat(document.getElementById('height_ft').value);
      const inch = parseFloat(document.getElementById('height_in').value);
      if (isNaN(ft) && isNaN(inch)) return 0;
      return ((isNaN(ft)?0:ft) * 0.3048) + ((isNaN(inch)?0:inch) * 0.0254);
    }
    function add(a,b){ return { p:a.p+b.p, c:a.c+b.c, f:a.f+b.f, kcal:a.kcal+b.kcal }; }

    // ========= Toggle inputs =========
    function toggleHeightInput(){
      const u = document.getElementById('height_unit').value;
      document.getElementById('height_cm_input').style.display = (u === 'cm') ? 'grid' : 'none';
      document.getElementById('height_ft_in_input').style.display = (u === 'ft_in') ? 'grid' : 'none';
    }
    function toggleWeightInput(){
      const u = document.getElementById('weight_unit').value;
      document.getElementById('weight_kg_input').style.display = (u === 'kg') ? 'grid' : 'none';
      document.getElementById('weight_lbs_input').style.display = (u === 'lbs') ? 'grid' : 'none';
    }

    // ========= Activity hint (Calculator.net-like mapping) =========
    function updateActivityHint(){
      const v = document.getElementById('activity_level').value;
      const map = {
        "1.2":   "Sedentary: little or no exercise",
        "1.375": "Light: exercise 1–3 times/week",
        "1.55":  "Moderate: exercise 4–5 times/week",
        "1.725": "Very Active: intense exercise 6–7 times/week",
        "1.9":   "Extra Active: very intense daily or physical job"
      };
      document.getElementById('activityHint').innerText = map[v] || "";
    }

    // ========= Macro tables =========
    // NOTE: keep your earlier meat estimates; you can swap later with full FoodData Central if you want.
    const meats = {
      chicken_breast:     { pPerGram: 0.31, fPerGram: 0.00, cPerGram: 0 },
      chicken_thigh:      { pPerGram: 0.26, fPerGram: 0.08, cPerGram: 0 },
      chicken_wings:      { pPerGram: 0.30, fPerGram: 0.00, cPerGram: 0 },
      chicken_drumsticks: { pPerGram: 0.28, fPerGram: 0.10, cPerGram: 0 },
      mutton:             { pPerGram: 0.25, fPerGram: 0.20, cPerGram: 0 },
      duck:               { pPerGram: 0.23, fPerGram: 0.28, cPerGram: 0 },
      pigeon:             { pPerGram: 0.25, fPerGram: 0.15, cPerGram: 0 },
      quail:              { pPerGram: 0.22, fPerGram: 0.12, cPerGram: 0 },
      beef:               { pPerGram: 0.26, fPerGram: 0.15, cPerGram: 0 },
      fish:               { pPerGram: 0.22, fPerGram: 0.05, cPerGram: 0 },
      shrimp:             { pPerGram: 0.24, fPerGram: 0.02, cPerGram: 0 },
      dried_fish:         { pPerGram: 0.60, fPerGram: 0.05, cPerGram: 0 }
    };

    const simple = {
      egg:            { p:6,   c:0.6, f:5   },
      protein_shake:  { p:25,  c:3,   f:2   },
      bread_slice:    { p:3,   c:15,  f:1.2 },
      salad_cup:      { p:0.5, c:2,   f:0.1 },
      veg_cup:        { p:2,   c:5,   f:0.2 },
      milk_tea_cup:   { p:0,   c:10,  f:0   },
      chocolate_cube: { p:1,   c:15,  f:5   },
      cake_slice:     { p:3,   c:30,  f:10  },
      soda_can:       { p:0,   c:40,  f:0   }
    };

    // Oils/Fats per TBSP (approx)
    const fatsPerTbsp = {
      olive_oil:   { p:0,    c:0,    f:13.5,  kcal:119 },
      soybean_oil: { p:0,    c:0,    f:13.6,  kcal:120 },
      butter:      { p:0.12, c:0.01, f:11.36, kcal:100 },
      ghee:        { p:0.04, c:0,    f:12.7,  kcal:112 }
    };

    // Nuts per 100g (approx)
    const nutsPer100g = {
      almonds:    { p:21.2,  c:21.6,  f:49.9,  kcal:579 },
      peanuts:    { p:25.8,  c:16.1,  f:49.2,  kcal:567 },
      cashews:    { p:18.22, c:30.19, f:43.85, kcal:553 },
      pistachios: { p:20.16, c:27.17, f:45.32, kcal:560 },
      walnuts:    { p:15.23, c:13.71, f:65.21, kcal:654 },
      hazelnuts:  { p:14.95, c:16.70, f:60.75, kcal:628 }
    };

    // Fruits per 100g (approx)
    const fruitsPer100g = {
      banana:     { p:1.29, c:26.95, f:0.39, kcal:105/1.18 },  // based on 118g=105 kcal; carbs/protein/fat also for 118g
      apple:      { p:0.47, c:25.13, f:0.31, kcal:95/1.82 },   // based on 182g=95 kcal
      orange:     { p:1.23, c:15.39, f:0.16, kcal:62/1.31 },   // based on 131g=62 kcal
      mango:      { p:0.84, c:28.05, f:0.45, kcal:107/1.65 },  // based on 165g=107 kcal
      grapes:     { p:0.72, c:18.10, f:0.16, kcal:69/1.00 },
      papaya:     { p:0.47, c:10.82, f:0.26, kcal:43/1.00 },
      watermelon: { p:0.61, c:7.55,  f:0.15, kcal:30/1.00 }
    };

    // Dates (support grams & pieces)
    // Generic per piece reference: 1 date (8g) => 23 kcal, carbs 6g, protein 0.2g, fat 0g :contentReference[oaicite:9]{index=9}
    // Ajwa example: 20g => 50 kcal, carbs 14g, protein 0g, fat 0g :contentReference[oaicite:10]{index=10}
    // Sukkari per 100g reference: 268 kcal, carbs 62g, protein 2g, fat 0g :contentReference[oaicite:11]{index=11}
    const datesRefs = {
      generic: { per100g: { p:2.5, c:75.0, f:0.4, kcal:282 }, defaultPieceG: 8 }, // aligned with ~8g/date
      ajwa:    { per100g: { p:0.0, c:70.0, f:0.0, kcal:250 }, defaultPieceG: 10 }, // derived from 20g => 50 kcal, 14g carbs
      sukkari: { per100g: { p:2.0, c:62.0, f:0.0, kcal:268 }, defaultPieceG: 10 }  // per 100g
    };

    const yogurtPer100g = { p:3.47, c:4.66, f:3.25, kcal:61 };
    const peanutButterPerTbsp = { p:3.9, c:2.7, f:8.7, kcal:97 };

    // Meals: use per 100g derived from previous "per cup" approximations (tunable).
    // You can refine later with your preferred dataset.
    const mealsPer100g = {
      biryani:   { p:4.54, c:13.73, f:2.81, kcal:99.4 },  // ~per 100g
      khichuri:  { p:1.75, c:9.79,  f:0.51, kcal:50.0 },
      friedrice: { p:3.56, c:11.91, f:3.53, kcal:95.1 }
    };

    // Rice cooked per 100g (simple approximation)
    const riceCookedPer100g = { p:2.7, c:28.0, f:0.3, kcal:130 };

    // ========= Dynamic Multi-Row Builders =========
    function makeSelect(options, value){
      const s = document.createElement('select');
      options.forEach(([v, t])=>{
        const o = document.createElement('option');
        o.value = v; o.textContent = t;
        s.appendChild(o);
      });
      if (value) s.value = value;
      return s;
    }

    function addDateRow(prefType="generic"){
      const wrap = document.createElement('div');
      wrap.className = "dynRow";

      const typeSel = makeSelect([
        ["generic","Generic date"],
        ["ajwa","Ajwa (small)"],
        ["sukkari","Sukkari (small)"]
      ], prefType);

      const unitSel = makeSelect([
        ["grams","grams"],
        ["pieces","pieces"]
      ], "pieces");

      const amt = document.createElement('input');
      amt.type="number"; amt.min="0"; amt.placeholder="Amount";

      const remove = document.createElement('button');
      remove.type="button"; remove.className="removeBtn"; remove.textContent="×";
      remove.onclick = ()=>{ wrap.remove(); scheduleCalc(); };

      const typeBox = document.createElement('div');
      typeBox.className="span2";
      typeBox.innerHTML = `<label>Date type</label>`;
      typeBox.appendChild(typeSel);

      const unitBox = document.createElement('div');
      unitBox.innerHTML = `<label>Unit</label>`;
      unitBox.appendChild(unitSel);

      const amtBox = document.createElement('div');
      amtBox.innerHTML = `<label>Amount</label>`;
      amtBox.appendChild(amt);

      wrap.appendChild(typeBox);
      wrap.appendChild(unitBox);
      wrap.appendChild(amtBox);
      wrap.appendChild(remove);

      [typeSel, unitSel, amt].forEach(el=>{
        el.addEventListener("input", scheduleCalc);
        el.addEventListener("change", scheduleCalc);
      });

      document.getElementById('dateRows').appendChild(wrap);
    }

    function addFruitRow(){
      const wrap = document.createElement('div');
      wrap.className = "dynRow";

      const fruitSel = makeSelect([
        ["banana","Banana"],
        ["apple","Apple"],
        ["orange","Orange"],
        ["mango","Mango"],
        ["grapes","Grapes"],
        ["papaya","Papaya"],
        ["watermelon","Watermelon"]
      ], "banana");

      const sizeSel = makeSelect([
        ["custom","Custom grams"],
        ["banana_med","Medium banana (~118g)"],
        ["apple_med","Medium apple (~182g)"],
        ["orange_med","Orange (~131g)"],
        ["mango_cup","Mango 1 cup (~165g)"]
      ], "custom");

      const grams = document.createElement('input');
      grams.type="number"; grams.min="0"; grams.placeholder="grams";

      const remove = document.createElement('button');
      remove.type="button"; remove.className="removeBtn"; remove.textContent="×";
      remove.onclick = ()=>{ wrap.remove(); scheduleCalc(); };

      const fruitBox = document.createElement('div');
      fruitBox.className="span2";
      fruitBox.innerHTML = `<label>Fruit</label>`;
      fruitBox.appendChild(fruitSel);

      const sizeBox = document.createElement('div');
      sizeBox.innerHTML = `<label>Size preset</label>`;
      sizeBox.appendChild(sizeSel);

      const gramsBox = document.createElement('div');
      gramsBox.innerHTML = `<label>Grams</label>`;
      gramsBox.appendChild(grams);

      function applySizePreset(){
        const v = sizeSel.value;
        if (v === "banana_med") grams.value = 118;
        else if (v === "apple_med") grams.value = 182;
        else if (v === "orange_med") grams.value = 131;
        else if (v === "mango_cup") grams.value = 165;
        scheduleCalc();
      }

      sizeSel.addEventListener("change", applySizePreset);
      fruitSel.addEventListener("change", scheduleCalc);
      grams.addEventListener("input", scheduleCalc);

      wrap.appendChild(fruitBox);
      wrap.appendChild(sizeBox);
      wrap.appendChild(gramsBox);
      wrap.appendChild(remove);

      document.getElementById('fruitRows').appendChild(wrap);
    }

    function addOilRow(){
      const wrap = document.createElement('div');
      wrap.className = "dynRow";

      const oilSel = makeSelect([
        ["olive_oil","Olive oil (tbsp)"],
        ["soybean_oil","Soybean oil (tbsp)"],
        ["butter","Butter (tbsp)"],
        ["ghee","Ghee (tbsp)"]
      ], "olive_oil");

      const tbsp = document.createElement('input');
      tbsp.type="number"; tbsp.min="0"; tbsp.placeholder="tbsp";

      const spacer = document.createElement('div');
      spacer.className="span2";
      spacer.innerHTML = `<label> </label><div class="miniHelp">You can add butter + ghee + oils together.</div>`;

      const remove = document.createElement('button');
      remove.type="button"; remove.className="removeBtn"; remove.textContent="×";
      remove.onclick = ()=>{ wrap.remove(); scheduleCalc(); };

      const oilBox = document.createElement('div');
      oilBox.className="span2";
      oilBox.innerHTML = `<label>Oil/Fat</label>`;
      oilBox.appendChild(oilSel);

      const tbspBox = document.createElement('div');
      tbspBox.innerHTML = `<label>Amount</label>`;
      tbspBox.appendChild(tbsp);

      wrap.appendChild(oilBox);
      wrap.appendChild(tbspBox);
      wrap.appendChild(spacer);
      wrap.appendChild(remove);

      oilSel.addEventListener("change", scheduleCalc);
      tbsp.addEventListener("input", scheduleCalc);

      document.getElementById('oilRows').appendChild(wrap);
    }

    function addNutRow(){
      const wrap = document.createElement('div');
      wrap.className = "dynRow";

      const nutSel = makeSelect([
        ["almonds","Almonds"],
        ["peanuts","Peanuts"],
        ["cashews","Cashews"],
        ["pistachios","Pistachios"],
        ["walnuts","Walnuts"],
        ["hazelnuts","Hazelnuts"]
      ], "almonds");

      const grams = document.createElement('input');
      grams.type="number"; grams.min="0"; grams.placeholder="grams";

      const spacer = document.createElement('div');
      spacer.className="span2";
      spacer.innerHTML = `<label> </label><div class="miniHelp">Add multiple nuts rows if you ate a mix.</div>`;

      const remove = document.createElement('button');
      remove.type="button"; remove.className="removeBtn"; remove.textContent="×";
      remove.onclick = ()=>{ wrap.remove(); scheduleCalc(); };

      const nutBox = document.createElement('div');
      nutBox.className="span2";
      nutBox.innerHTML = `<label>Nuts</label>`;
      nutBox.appendChild(nutSel);

      const gramsBox = document.createElement('div');
      gramsBox.innerHTML = `<label>Amount</label>`;
      gramsBox.appendChild(grams);

      wrap.appendChild(nutBox);
      wrap.appendChild(gramsBox);
      wrap.appendChild(spacer);
      wrap.appendChild(remove);

      nutSel.addEventListener("change", scheduleCalc);
      grams.addEventListener("input", scheduleCalc);

      document.getElementById('nutRows').appendChild(wrap);
    }

    // ========= Section computations =========
    function computeChickenSection(){
      const breast = num('chicken_breast');
      const thigh  = num('chicken_thigh');
      const wings  = num('chicken_wings');
      const drum   = num('chicken_drumsticks');
      const p = (breast*meats.chicken_breast.pPerGram)+(thigh*meats.chicken_thigh.pPerGram)+(wings*meats.chicken_wings.pPerGram)+(drum*meats.chicken_drumsticks.pPerGram);
      const f = (breast*meats.chicken_breast.fPerGram)+(thigh*meats.chicken_thigh.fPerGram)+(wings*meats.chicken_wings.fPerGram)+(drum*meats.chicken_drumsticks.fPerGram);
      const c = 0;
      const kcal = (p*4)+(c*4)+(f*9);
      return {p,c,f,kcal};
    }

    function computeOtherProteinSection(){
      let total = {p:0,c:0,f:0,kcal:0};
      const scoops = num('protein_shake');
      total = add(total,{p:scoops*simple.protein_shake.p,c:scoops*simple.protein_shake.c,f:scoops*simple.protein_shake.f,kcal:(scoops*simple.protein_shake.p*4)+(scoops*simple.protein_shake.c*4)+(scoops*simple.protein_shake.f*9)});
      const eggs = num('egg');
      total = add(total,{p:eggs*simple.egg.p,c:eggs*simple.egg.c,f:eggs*simple.egg.f,kcal:(eggs*simple.egg.p*4)+(eggs*simple.egg.c*4)+(eggs*simple.egg.f*9)});
      ["mutton","duck","pigeon","quail","beef","fish","shrimp","dried_fish"].forEach(id=>{
        const g = num(id);
        const m = meats[id];
        const p = g*m.pPerGram, f = g*m.fPerGram, c = g*m.cPerGram;
        total = add(total,{p,c,f,kcal:(p*4)+(c*4)+(f*9)});
      });
      const yg = num('yogurt');
      total = add(total,{p:(yg/100)*yogurtPer100g.p,c:(yg/100)*yogurtPer100g.c,f:(yg/100)*yogurtPer100g.f,kcal:(yg/100)*yogurtPer100g.kcal});
      return total;
    }

    function computeCarbsBasicsAndDates(){
      let total = {p:0,c:0,f:0,kcal:0};

      // Rice (grams or plate)
      let riceGrams = 0;
      const riceMode = document.getElementById('rice_mode').value;
      if(riceMode === "grams"){
        riceGrams = num('rice_grams');
      } else {
        const frac = parseFloat(document.getElementById('rice_plate_fraction').value) || 0;
        const plateG = num('rice_plate_grams');
        riceGrams = frac * plateG;
      }
      total = add(total,{
        p:(riceGrams/100)*riceCookedPer100g.p,
        c:(riceGrams/100)*riceCookedPer100g.c,
        f:(riceGrams/100)*riceCookedPer100g.f,
        kcal:(riceGrams/100)*riceCookedPer100g.kcal
      });

      const bread = num('bread');
      total = add(total,{p:bread*simple.bread_slice.p,c:bread*simple.bread_slice.c,f:bread*simple.bread_slice.f,kcal:(bread*simple.bread_slice.p*4)+(bread*simple.bread_slice.c*4)+(bread*simple.bread_slice.f*9)});

      const veg = num('vegetables');
      total = add(total,{p:veg*simple.veg_cup.p,c:veg*simple.veg_cup.c,f:veg*simple.veg_cup.f,kcal:(veg*simple.veg_cup.p*4)+(veg*simple.veg_cup.c*4)+(veg*simple.veg_cup.f*9)});

      const salad = num('salad');
      total = add(total,{p:salad*simple.salad_cup.p,c:salad*simple.salad_cup.c,f:salad*simple.salad_cup.f,kcal:(salad*simple.salad_cup.p*4)+(salad*simple.salad_cup.c*4)+(salad*simple.salad_cup.f*9)});

      const pb = num('peanut_butter');
      total = add(total,{p:pb*peanutButterPerTbsp.p,c:pb*peanutButterPerTbsp.c,f:pb*peanutButterPerTbsp.f,kcal:pb*peanutButterPerTbsp.kcal});

      // Dates rows
      const rows = document.querySelectorAll("#dateRows .dynRow");
      rows.forEach(r=>{
        const typeSel = r.querySelector("select");                 // first select
        const unitSel = r.querySelectorAll("select")[1];           // second select
        const amtEl   = r.querySelector("input");                  // amount
        const type = typeSel.value;
        const unit = unitSel.value;
        const amt = parseFloat(amtEl.value) || 0;
        const ref = datesRefs[type] || datesRefs.generic;

        let grams = 0;
        if(unit === "grams") grams = amt;
        else grams = amt * (ref.defaultPieceG || 8);

        total = add(total,{
          p:(grams/100)*ref.per100g.p,
          c:(grams/100)*ref.per100g.c,
          f:(grams/100)*ref.per100g.f,
          kcal:(grams/100)*ref.per100g.kcal
        });
      });

      return total;
    }

    function computeFruitsMulti(){
      let total = {p:0,c:0,f:0,kcal:0};
      const rows = document.querySelectorAll("#fruitRows .dynRow");
      rows.forEach(r=>{
        const fruit = r.querySelector("select").value;
        const grams = parseFloat(r.querySelectorAll("input")[0].value) || 0;
        const f = fruitsPer100g[fruit] || {p:0,c:0,f:0,kcal:0};
        total = add(total,{p:(grams/100)*f.p,c:(grams/100)*f.c,f:(grams/100)*f.f,kcal:(grams/100)*f.kcal});
      });
      return total;
    }

    function computeOilsMulti(){
      let total = {p:0,c:0,f:0,kcal:0};
      const rows = document.querySelectorAll("#oilRows .dynRow");
      rows.forEach(r=>{
        const oil = r.querySelector("select").value;
        const tbsp = parseFloat(r.querySelector("input").value) || 0;
        const o = fatsPerTbsp[oil] || {p:0,c:0,f:0,kcal:0};
        total = add(total,{p:tbsp*o.p,c:tbsp*o.c,f:tbsp*o.f,kcal:tbsp*o.kcal});
      });
      return total;
    }

    function computeNutsMulti(){
      let total = {p:0,c:0,f:0,kcal:0};
      const rows = document.querySelectorAll("#nutRows .dynRow");
      rows.forEach(r=>{
        const nut = r.querySelector("select").value;
        const grams = parseFloat(r.querySelector("input").value) || 0;
        const n = nutsPer100g[nut] || {p:0,c:0,f:0,kcal:0};
        total = add(total,{p:(grams/100)*n.p,c:(grams/100)*n.c,f:(grams/100)*n.f,kcal:(grams/100)*n.kcal});
      });
      return total;
    }

    function computeMeals(){
      let total = {p:0,c:0,f:0,kcal:0};
      const plateG = num('meal_plate_grams') || 350;

      function gramsFromMode(modeSelId, gramsInputId, plateSelId){
        const mode = document.getElementById(modeSelId).value;
        if(mode === "grams") return num(gramsInputId);
        const frac = parseFloat(document.getElementById(plateSelId).value) || 0;
        return frac * plateG;
      }

      const biryaniG  = gramsFromMode("biryani_mode","meal_biryani_grams","meal_biryani_plate");
      const khichuriG = gramsFromMode("khichuri_mode","meal_khichuri_grams","meal_khichuri_plate");
      const friedG    = gramsFromMode("friedrice_mode","meal_friedrice_grams","meal_friedrice_plate");

      total = add(total,{p:(biryaniG/100)*mealsPer100g.biryani.p,c:(biryaniG/100)*mealsPer100g.biryani.c,f:(biryaniG/100)*mealsPer100g.biryani.f,kcal:(biryaniG/100)*mealsPer100g.biryani.kcal});
      total = add(total,{p:(khichuriG/100)*mealsPer100g.khichuri.p,c:(khichuriG/100)*mealsPer100g.khichuri.c,f:(khichuriG/100)*mealsPer100g.khichuri.f,kcal:(khichuriG/100)*mealsPer100g.khichuri.kcal});
      total = add(total,{p:(friedG/100)*mealsPer100g.friedrice.p,c:(friedG/100)*mealsPer100g.friedrice.c,f:(friedG/100)*mealsPer100g.friedrice.f,kcal:(friedG/100)*mealsPer100g.friedrice.kcal});

      return total;
    }

    function computeSnacksDrinks(){
      let total = {p:0,c:0,f:0,kcal:0};
      const mt = num('milk_tea');
      total = add(total,{p:mt*simple.milk_tea_cup.p,c:mt*simple.milk_tea_cup.c,f:mt*simple.milk_tea_cup.f,kcal:(mt*simple.milk_tea_cup.p*4)+(mt*simple.milk_tea_cup.c*4)+(mt*simple.milk_tea_cup.f*9)});
      const ch = num('chocolate');
      total = add(total,{p:ch*simple.chocolate_cube.p,c:ch*simple.chocolate_cube.c,f:ch*simple.chocolate_cube.f,kcal:(ch*simple.chocolate_cube.p*4)+(ch*simple.chocolate_cube.c*4)+(ch*simple.chocolate_cube.f*9)});
      const ck = num('cake');
      total = add(total,{p:ck*simple.cake_slice.p,c:ck*simple.cake_slice.c,f:ck*simple.cake_slice.f,kcal:(ck*simple.cake_slice.p*4)+(ck*simple.cake_slice.c*4)+(ck*simple.cake_slice.f*9)});
      const soda = num('carbonated_beverage');
      total = add(total,{p:soda*simple.soda_can.p,c:soda*simple.soda_can.c,f:soda*simple.soda_can.f,kcal:(soda*simple.soda_can.p*4)+(soda*simple.soda_can.c*4)+(soda*simple.soda_can.f*9)});
      return total;
    }

    // ========= BMI / Protein / TDEE =========
    function calcBMI(){
      const h = getHeightMeters(), w = getWeightKg();
      if (h <= 0 || w <= 0) return 0;
      return w / (h*h);
    }
    function calcRecommendedRangeFromWeight(weightKg){
      return { min: weightKg * 1.5, max: weightKg * 2.0 };
    }
    function formatRange(min,max){
      return `${Math.round(min)}–${Math.round(max)} g`;
    }

    function calcTDEEAndTarget(){
      const w = getWeightKg();
      const h = getHeightMeters();
      const age = num('age');
      const activity = parseFloat(document.getElementById('activity_level').value);
      const goal = document.getElementById('goal').value;
      if (w <= 0 || h <= 0 || age <= 0) return { tdee:0, target:0 };

      // Mifflin-St Jeor (male baseline)
      const bmr = (10 * w) + (6.25 * h * 100) - (5 * age) + 5;
      const tdee = bmr * activity;

      let target = tdee;
      if (goal === 'deficit') target = tdee - 500;
      if (goal === 'gain')    target = tdee + 400;

      return { tdee, target };
    }

    function updateBMIUI(bmi){
      const bmiEl = document.getElementById('bmi');
      const marker = document.getElementById('bmiMarker');
      const msg = document.getElementById('bmiMsg');

      bmiEl.innerText = bmi ? bmi.toFixed(2) : "0";

      // Map BMI to bar (0–40 clipped)
      const clip = Math.max(0, Math.min(40, bmi || 0));
      const pct = (clip / 40) * 100;
      marker.style.left = `${pct}%`;

      let status = "Enter height & weight to see your BMI status.";
      if (bmi > 0){
        if (bmi < 18.5) status = "Underweight: consider a calorie surplus + protein target + strength training (if medically appropriate).";
        else if (bmi < 25) status = "Normal range: maintain or lean-gain depending on your goal.";
        else if (bmi < 30) status = "Overweight: if goal is fat loss, aim for a moderate deficit with high protein.";
        else status = "Obese range: prioritize health-first fat loss plan; consult a clinician if needed.";
      }
      msg.innerText = status;
    }

    // ========= Chart (create ONCE, update data only) =========
    let macroChart;
    function initChart(){
      const ctx = document.getElementById('macroChart').getContext('2d');
      macroChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ['Protein', 'Fat', 'Carbohydrates'],
          datasets: [{
            label: 'Macro Breakdown (g)',
            data: [0,0,0],
            backgroundColor: ['#06D6A0', '#EF476F', '#FFD166'] // none blue
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          animation: false,
          plugins: { legend: { display: false } },
          scales: { y: { beginAtZero: true } }
        }
      });
    }
    function updateChartData(protein,fat,carbs){
      macroChart.data.datasets[0].data = [protein, fat, carbs];
      macroChart.update('none');
    }

    // ========= Main calculation (debounced) =========
    let userManuallySetTargetProtein = false;
    let calcTimer = null;
    function scheduleCalc(){
      if(calcTimer) clearTimeout(calcTimer);
      calcTimer = setTimeout(calculateAll, 80);
    }

    function calculateAll(){
      toggleHeightInput();
      toggleWeightInput();
      updateActivityHint();

      // toggle rice
      const rm = document.getElementById('rice_mode').value;
      document.getElementById('rice_grams_row').style.display = (rm === "grams") ? "grid" : "none";
      document.getElementById('rice_plate_row').style.display = (rm === "plate") ? "grid" : "none";

      // toggle meals modes
      const bm = document.getElementById('biryani_mode').value;
      document.getElementById('biryani_grams_box').style.display = (bm === "grams") ? "block" : "none";
      document.getElementById('biryani_plate_box').style.display = (bm === "plate") ? "block" : "none";

      const km = document.getElementById('khichuri_mode').value;
      document.getElementById('khichuri_grams_box').style.display = (km === "grams") ? "block" : "none";
      document.getElementById('khichuri_plate_box').style.display = (km === "plate") ? "block" : "none";

      const fm = document.getElementById('friedrice_mode').value;
      document.getElementById('friedrice_grams_box').style.display = (fm === "grams") ? "block" : "none";
      document.getElementById('friedrice_plate_box').style.display = (fm === "plate") ? "block" : "none";

      // BMI + UI
      const bmi = calcBMI();
      updateBMIUI(bmi);

      // Recommended protein + smart target default
      const w = getWeightKg();
      const { min, max } = (w>0) ? calcRecommendedRangeFromWeight(w) : {min:0,max:0};
      document.getElementById('recommended_protein').innerText = (w>0) ? formatRange(min,max) : "0";
      if (!userManuallySetTargetProtein && w > 0){
        const suggested = min + 0.60*(max - min);
        document.getElementById('target_protein').value = roundUpToNearest10(suggested);
      }

      // TDEE + target
      const { tdee, target } = calcTDEEAndTarget();
      document.getElementById('tdee').innerText = tdee.toFixed(0);
      document.getElementById('targetCalories').innerText = target.toFixed(0);

      // Sections
      const chicken = computeChickenSection();
      const otherP  = computeOtherProteinSection();
      const carbsB  = computeCarbsBasicsAndDates();
      const fruits  = computeFruitsMulti();
      const oils    = computeOilsMulti();
      const nuts    = computeNutsMulti();
      const meals   = computeMeals();
      const snacks  = computeSnacksDrinks();

      // Previews
      document.getElementById('ch_protein').innerText = round2(chicken.p);
      document.getElementById('ch_fat').innerText     = round2(chicken.f);
      document.getElementById('ch_carbs').innerText   = round2(chicken.c);
      document.getElementById('ch_cals').innerText    = round2(chicken.kcal);

      document.getElementById('op_protein').innerText = round2(otherP.p);
      document.getElementById('op_fat').innerText     = round2(otherP.f);
      document.getElementById('op_carbs').innerText   = round2(otherP.c);
      document.getElementById('op_cals').innerText    = round2(otherP.kcal);

      const carbsAll = add(carbsB, fruits);
      document.getElementById('cb_protein').innerText = round2(carbsAll.p);
      document.getElementById('cb_fat').innerText     = round2(carbsAll.f);
      document.getElementById('cb_carbs').innerText   = round2(carbsAll.c);
      document.getElementById('cb_cals').innerText    = round2(carbsAll.kcal);

      document.getElementById('fr_protein').innerText = round2(fruits.p);
      document.getElementById('fr_fat').innerText     = round2(fruits.f);
      document.getElementById('fr_carbs').innerText   = round2(fruits.c);
      document.getElementById('fr_cals').innerText    = round2(fruits.kcal);

      document.getElementById('fn_protein').innerText = round2(oils.p);
      document.getElementById('fn_fat').innerText     = round2(oils.f);
      document.getElementById('fn_carbs').innerText   = round2(oils.c);
      document.getElementById('fn_cals').innerText    = round2(oils.kcal);

      document.getElementById('nt_protein').innerText = round2(nuts.p);
      document.getElementById('nt_fat').innerText     = round2(nuts.f);
      document.getElementById('nt_carbs').innerText   = round2(nuts.c);
      document.getElementById('nt_cals').innerText    = round2(nuts.kcal);

      document.getElementById('ml_protein').innerText = round2(meals.p);
      document.getElementById('ml_fat').innerText     = round2(meals.f);
      document.getElementById('ml_carbs').innerText   = round2(meals.c);
      document.getElementById('ml_cals').innerText    = round2(meals.kcal);

      document.getElementById('sd_protein').innerText = round2(snacks.p);
      document.getElementById('sd_fat').innerText     = round2(snacks.f);
      document.getElementById('sd_carbs').innerText   = round2(snacks.c);
      document.getElementById('sd_cals').innerText    = round2(snacks.kcal);

      // Totals
      const total = add(add(add(add(add(add(add(chicken, otherP), carbsB), fruits), oils), nuts), meals), snacks);

      document.getElementById('totalProtein').innerText  = round2(total.p);
      document.getElementById('totalFat').innerText      = round2(total.f);
      document.getElementById('totalCarbs').innerText    = round2(total.c);
      document.getElementById('totalCalories').innerText = round2(total.kcal);

      // Protein balance
      const tp = clampNonNegative(num('target_protein'));
      const balance = tp - total.p;
      document.getElementById('proteinBalance').innerText = round2(balance);

      const box = document.getElementById('proteinBalanceBox');
      box.classList.remove('danger','good');
      if (balance > 0.01) box.classList.add('danger'); else box.classList.add('good');

      // Chart update (no jump)
      if (macroChart) updateChartData(total.p, total.f, total.c);
    }

    // ========= PDF + Reset =========
    function saveReport(){
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      doc.setFontSize(18);
      doc.text("Nowshad's Macro Calculator Report", 10, 12);
      const date = new Date().toLocaleString();
      doc.setFontSize(11);
      doc.text(`Generated: ${date}`, 10, 20);
      doc.setFontSize(12);
      doc.text(`Name: ${document.getElementById('name').value || "-"}`, 10, 34);
      doc.text(`Age: ${document.getElementById('age').value || "-"}`, 10, 42);
      doc.text(`BMI: ${document.getElementById('bmi').innerText}`, 10, 50);
      doc.text(`Protein: ${document.getElementById('totalProtein').innerText} g`, 10, 60);
      doc.text(`Fat: ${document.getElementById('totalFat').innerText} g`, 10, 68);
      doc.text(`Carbs: ${document.getElementById('totalCarbs').innerText} g`, 10, 76);
      doc.text(`Calories: ${document.getElementById('totalCalories').innerText} kcal`, 10, 84);
      doc.text(`TDEE: ${document.getElementById('tdee').innerText} kcal`, 10, 96);
      doc.text(`Target Calories: ${document.getElementById('targetCalories').innerText} kcal`, 10, 104);
      doc.save("Nowshad_Macro_Calculator_Report.pdf");
    }

    function resetAll(){
      userManuallySetTargetProtein = false;

      // clear simple inputs
      [
        "name","age","height_cm","height_ft","height_in","weight_kg","weight_lbs","target_protein",
        "chicken_breast","chicken_thigh","chicken_wings","chicken_drumsticks",
        "protein_shake","egg","mutton","duck","pigeon","quail","beef","fish","shrimp","dried_fish","yogurt",
        "bread","vegetables","salad","peanut_butter",
        "milk_tea","chocolate","cake","carbonated_beverage",
        "rice_grams","rice_plate_grams","meal_plate_grams",
        "meal_biryani_grams","meal_khichuri_grams","meal_friedrice_grams"
      ].forEach(id=>{
        const el = document.getElementById(id);
        if(el) el.value = "";
      });

      // reset selects
      document.getElementById('height_unit').value = "cm";
      document.getElementById('weight_unit').value = "kg";
      document.getElementById('activity_level').value = "1.2";
      document.getElementById('goal').value = "maintenance";
      document.getElementById('rice_mode').value = "grams";
      document.getElementById('biryani_mode').value = "grams";
      document.getElementById('khichuri_mode').value = "grams";
      document.getElementById('friedrice_mode').value = "grams";

      document.getElementById('rice_plate_grams').value = 250;
      document.getElementById('meal_plate_grams').value = 350;

      // clear dynamic rows
      document.getElementById('dateRows').innerHTML = "";
      document.getElementById('fruitRows').innerHTML = "";
      document.getElementById('oilRows').innerHTML = "";
      document.getElementById('nutRows').innerHTML = "";

      // add one row each as defaults
      addDateRow("generic");
      addFruitRow();
      addOilRow();
      addNutRow();

      scheduleCalc();
    }

    // ========= Bind events =========
    function bindAutoCalc(){
      document.addEventListener("input", (e)=>{
        if(e.target && e.target.id === "target_protein") userManuallySetTargetProtein = true;
      });

      // all inputs + selects auto-calc
      document.querySelectorAll("input, select").forEach(el=>{
        el.addEventListener("input", scheduleCalc);
        el.addEventListener("change", scheduleCalc);
      });

      document.getElementById("btnCalculate").addEventListener("click", (e)=>{ e.preventDefault(); calculateAll(); });
      document.getElementById("btnSave").addEventListener("click", (e)=>{ e.preventDefault(); saveReport(); });
      document.getElementById("btnReset").addEventListener("click", (e)=>{ e.preventDefault(); resetAll(); });

      document.getElementById("addDateRow").addEventListener("click", ()=>{ addDateRow("generic"); scheduleCalc(); });
      document.getElementById("addFruitRow").addEventListener("click", ()=>{ addFruitRow(); scheduleCalc(); });
      document.getElementById("addOilRow").addEventListener("click", ()=>{ addOilRow(); scheduleCalc(); });
      document.getElementById("addNutRow").addEventListener("click", ()=>{ addNutRow(); scheduleCalc(); });
    }

    // ========= Init =========
    toggleHeightInput();
    toggleWeightInput();
    initChart();
    bindAutoCalc();

    // default dynamic rows
    addDateRow("generic");
    addFruitRow();
    addOilRow();
    addNutRow();

    calculateAll();
  </script>
</body>
</html>
