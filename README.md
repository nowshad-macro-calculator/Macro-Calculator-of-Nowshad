
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Nowshad's Macro Calculator</title>

  <!-- Google Font -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">

  <style>
    :root{
      --bg1:#0b4aa2;
      --bg2:#0a2d6b;
      --card:#ffffffee;
      --ink:#0b1630;
      --muted:#4b5875;
      --accent:#2f7cff;
      --accent2:#00c2ff;
      --danger:#e74c3c;
      --ok:#12b981;
      --shadow: 0 18px 40px rgba(0,0,0,.18);
      --radius: 16px;
    }

    * { box-sizing: border-box; }

    body {
      font-family: "Poppins", system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      margin: 0;
      min-height: 100vh;
      color: #eaf2ff;
      background:
        radial-gradient(1200px 700px at 15% 10%, rgba(47,124,255,.45), transparent 60%),
        radial-gradient(900px 600px at 85% 20%, rgba(0,194,255,.28), transparent 55%),
        linear-gradient(160deg, var(--bg1), var(--bg2));
      padding: 28px 14px;
    }

    .container {
      max-width: 860px;
      margin: 0 auto;
      padding: 22px 22px 18px;
      background: var(--card);
      color: var(--ink);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      border: 1px solid rgba(255,255,255,.35);
      backdrop-filter: blur(8px);
    }

    .titlebar{
      display:flex;
      align-items:flex-end;
      justify-content: space-between;
      gap: 10px;
      flex-wrap: wrap;
      margin-bottom: 12px;
    }

    h2 {
      margin: 0;
      font-size: 26px;
      letter-spacing: .2px;
      color: #061633;
    }
    .subtitle{
      margin: 0;
      font-size: 13px;
      color: var(--muted);
    }

    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 14px 14px;
      margin-top: 12px;
    }
    @media (max-width: 780px){
      .grid { grid-template-columns: 1fr; }
    }

    .section {
      grid-column: 1 / -1;
      margin-top: 4px;
      padding: 12px 14px;
      border-radius: 14px;
      background: rgba(47,124,255,.08);
      border: 1px solid rgba(47,124,255,.18);
    }
    .section h3{
      margin: 0 0 10px;
      font-size: 16px;
      color: #0b2a5a;
    }

    label {
      display: block;
      font-weight: 600;
      color: #1b2a4a;
      font-size: 13px;
      margin-bottom: 6px;
    }

    input, select, button {
      width: 100%;
      padding: 11px 12px;
      border: 1px solid rgba(10,45,107,.18);
      border-radius: 12px;
      font-size: 14px;
      outline: none;
      background: #fff;
      color: #101a31;
      transition: transform .06s ease, box-shadow .18s ease, border-color .18s ease;
    }

    input:focus, select:focus {
      border-color: rgba(47,124,255,.55);
      box-shadow: 0 0 0 4px rgba(47,124,255,.18);
    }

    .row2{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }
    @media (max-width: 420px){
      .row2{ grid-template-columns: 1fr; }
    }

    button {
      border: none;
      cursor: pointer;
      font-weight: 700;
      letter-spacing: .2px;
      color: white;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      box-shadow: 0 12px 22px rgba(47,124,255,.25);
    }
    button:hover { transform: translateY(-1px); }
    button:active { transform: translateY(0px); }

    .metrics{
      display:grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      margin-top: 10px;
    }
    @media (max-width: 780px){
      .metrics{ grid-template-columns: 1fr; }
    }

    .pill{
      border-radius: 14px;
      padding: 12px 12px;
      background: rgba(10,45,107,.05);
      border: 1px solid rgba(10,45,107,.10);
    }

    .pill .k{ font-size: 12px; color: var(--muted); margin: 0; }
    .pill .v{ font-size: 18px; font-weight: 700; margin: 4px 0 0; color: #081a3a; }

    .danger { color: var(--danger) !important; }
    .ok { color: var(--ok) !important; }

    .mini-note{
      margin: 6px 0 0;
      font-size: 12px;
      color: var(--muted);
      line-height: 1.35;
    }

    .butter-box{
      margin-top: 10px;
      padding: 12px;
      border-radius: 14px;
      background: rgba(0,194,255,.08);
      border: 1px solid rgba(0,194,255,.18);
    }
    .butter-macros{
      margin-top: 8px;
      display:flex;
      flex-wrap: wrap;
      gap: 8px 12px;
      font-size: 13px;
      color: #0b2a5a;
      font-weight: 600;
    }
    .butter-macros span{
      background: rgba(255,255,255,.75);
      border: 1px solid rgba(10,45,107,.10);
      padding: 6px 10px;
      border-radius: 999px;
    }

    .chart-container {
      margin-top: 14px;
      text-align: center;
      background: rgba(255,255,255,.6);
      border-radius: 16px;
      padding: 12px;
      border: 1px solid rgba(10,45,107,.10);
    }

    .disclaimer {
      font-size: 12px;
      color: #4d5b76;
      margin-top: 14px;
      text-align: center;
      line-height: 1.45;
    }
  </style>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>

<body>
  <div class="container">
    <div class="titlebar">
      <div>
        <h2>Nowshad's Macro Calculator</h2>
        <p class="subtitle">Fast macro totals + butter macro preview (tsp/gram)</p>
      </div>
      <div style="min-width:220px;">
        <button onclick="calculateMacros()">Calculate Macros</button>
      </div>
    </div>

    <div class="grid">
      <div>
        <label for="name">Name</label>
        <input type="text" id="name" placeholder="Enter your name">
      </div>
      <div>
        <label for="age">Age</label>
        <input type="number" id="age" placeholder="Enter your age">
      </div>

      <div class="section">
        <h3>Body Metrics</h3>

        <div class="row2">
          <div>
            <label for="height_unit">Height Unit</label>
            <select id="height_unit" onchange="toggleHeightInput()">
              <option value="cm">Centimeters (cm)</option>
              <option value="ft_in">Feet/Inches (ft/in)</option>
            </select>
          </div>
          <div>
            <label for="weight_unit">Weight Unit</label>
            <select id="weight_unit" onchange="toggleWeightInput()">
              <option value="kg">Kilograms (kg)</option>
              <option value="lbs">Pounds (lbs)</option>
            </select>
          </div>
        </div>

        <div id="height_cm_input" style="margin-top:10px;">
          <label for="height_cm">Height (cm)</label>
          <input type="number" id="height_cm" placeholder="Enter your height in cm">
        </div>

        <div id="height_ft_in_input" style="display:none; margin-top:10px;">
          <div class="row2">
            <div>
              <label for="height_ft">Height (ft)</label>
              <input type="number" id="height_ft" placeholder="Feet">
            </div>
            <div>
              <label for="height_in">Height (in)</label>
              <input type="number" id="height_in" placeholder="Inches">
            </div>
          </div>
        </div>

        <div id="weight_kg_input" style="margin-top:10px;">
          <label for="weight_kg">Weight (kg)</label>
          <input type="number" id="weight_kg" placeholder="Enter your weight in kg" oninput="calculateRecommendedProtein()">
        </div>

        <div id="weight_lbs_input" style="display:none; margin-top:10px;">
          <label for="weight_lbs">Weight (lbs)</label>
          <input type="number" id="weight_lbs" placeholder="Enter your weight in lbs" oninput="calculateRecommendedProtein()">
        </div>

        <div class="metrics">
          <div class="pill">
            <p class="k">BMI</p>
            <p class="v"><span id="bmi">0</span></p>
          </div>
          <div class="pill">
            <p class="k">Recommended Protein (g)</p>
            <p class="v"><span id="recommended_protein">0</span></p>
          </div>
          <div class="pill">
            <p class="k">Required Protein (g)</p>
            <p class="v"><span id="required_protein_display">0</span></p>
          </div>
        </div>

        <div style="margin-top:10px;">
          <label for="required_protein">Enter Required Protein (grams)</label>
          <input type="number" id="required_protein" placeholder="Enter required protein" oninput="updateRequiredProtein()">
          <p class="mini-note"><b>Tip:</b> If Protein Balance turns red, you’re under target.</p>
        </div>
      </div>

      <div class="section">
        <h3>Protein Shake & Eggs</h3>
        <div class="row2">
          <div>
            <label for="protein_shake">Protein Shake (scoops)</label>
            <input type="number" id="protein_shake" placeholder="Enter scoops">
          </div>
          <div>
            <label for="egg">Eggs (count)</label>
            <input type="number" id="egg" placeholder="Enter count">
          </div>
        </div>
      </div>

      <div class="section">
        <h3>Chicken Items</h3>
        <div class="row2">
          <div>
            <label for="chicken_breast">Chicken Breast (grams)</label>
            <input type="number" id="chicken_breast" placeholder="Enter grams">
          </div>
          <div>
            <label for="chicken_thigh">Chicken Thigh (grams)</label>
            <input type="number" id="chicken_thigh" placeholder="Enter grams">
          </div>
        </div>

        <div class="row2" style="margin-top:10px;">
          <div>
            <label for="chicken_wings">Chicken Wings (grams)</label>
            <input type="number" id="chicken_wings" placeholder="Enter grams">
          </div>
          <div>
            <label for="chicken_drumsticks">Chicken Drumsticks (grams)</label>
            <input type="number" id="chicken_drumsticks" placeholder="Enter grams">
          </div>
        </div>
      </div>

      <div class="section">
        <h3>Other Meats</h3>
        <div class="row2">
          <div>
            <label for="mutton">Mutton (grams)</label>
            <input type="number" id="mutton" placeholder="Enter grams">
          </div>
          <div>
            <label for="duck">Duck (grams)</label>
            <input type="number" id="duck" placeholder="Enter grams">
          </div>
        </div>

        <div class="row2" style="margin-top:10px;">
          <div>
            <label for="pigeon">Pigeon (grams)</label>
            <input type="number" id="pigeon" placeholder="Enter grams">
          </div>
          <div>
            <label for="quail">Quail (grams)</label>
            <input type="number" id="quail" placeholder="Enter grams">
          </div>
        </div>

        <div class="row2" style="margin-top:10px;">
          <div>
            <label for="beef">Beef (grams)</label>
            <input type="number" id="beef" placeholder="Enter grams">
          </div>
          <div>
            <label for="fish">Fish (grams)</label>
            <input type="number" id="fish" placeholder="Enter grams">
          </div>
        </div>

        <div class="row2" style="margin-top:10px;">
          <div>
            <label for="shrimp">Shrimp (grams)</label>
            <input type="number" id="shrimp" placeholder="Enter grams">
          </div>
          <div>
            <label for="dried_fish">Dried Fish/Bombay Duck (grams)</label>
            <input type="number" id="dried_fish" placeholder="Enter grams">
          </div>
        </div>
      </div>

      <div class="section">
        <h3>Carbs, Oils & Veg</h3>
        <div class="row2">
          <div>
            <label for="rice">Rice (cups)</label>
            <input type="number" id="rice" placeholder="Enter cups">
          </div>
          <div>
            <label for="bread">Bread (slices)</label>
            <input type="number" id="bread" placeholder="Enter slices">
          </div>
        </div>

        <div class="row2" style="margin-top:10px;">
          <div>
            <label for="oil">Oil (tablespoons)</label>
            <input type="number" id="oil" placeholder="Enter tablespoons">
          </div>
          <div>
            <label for="salad">Salad (cups)</label>
            <input type="number" id="salad" placeholder="Enter cups">
          </div>
        </div>

        <div style="margin-top:10px;">
          <label for="vegetables">Vegetables (cups)</label>
          <input type="number" id="vegetables" placeholder="Enter cups">
        </div>

        <!-- BUTTER: unit (tsp/gram) + instant macro display -->
        <div class="butter-box">
          <h3 style="margin:0 0 8px;">Butter (Instant Macro Preview)</h3>

          <div class="row2">
            <div>
              <label for="butter_unit">Butter Unit</label>
              <select id="butter_unit" onchange="updateButterMacros()">
                <option value="tsp">Teaspoon (tsp)</option>
                <option value="g">Gram (g)</option>
              </select>
            </div>
            <div>
              <label for="butter_qty">Butter Quantity</label>
              <input type="number" id="butter_qty" placeholder="Enter amount" oninput="updateButterMacros()">
            </div>
          </div>

          <div class="butter-macros" id="butter_macro_line">
            <span>Protein: 0.00 g</span>
            <span>Fat: 0.00 g</span>
            <span>Carbs: 0.00 g</span>
            <span>Calories: 0.00 kcal</span>
          </div>

          <p class="mini-note" style="margin-top:10px;">
            Butter macros are computed from <b>USDA/FoodData Central–derived</b> nutrition for <b>salted butter (1 tbsp = 14g)</b>.
          </p>
        </div>
      </div>

      <div class="section">
        <h3>Extras</h3>
        <div class="row2">
          <div>
            <label for="milk_tea">Milk Tea (cups)</label>
            <input type="number" id="milk_tea" placeholder="Enter cups">
          </div>
          <div>
            <label for="chocolate">Cube of Chocolate (count)</label>
            <input type="number" id="chocolate" placeholder="Enter count">
          </div>
        </div>
        <div class="row2" style="margin-top:10px;">
          <div>
            <label for="cake">Slice of Cake (count)</label>
            <input type="number" id="cake" placeholder="Enter count">
          </div>
          <div>
            <label for="carbonated_beverage">Carbonated Beverage (cans)</label>
            <input type="number" id="carbonated_beverage" placeholder="Enter cans">
          </div>
        </div>
      </div>

      <div class="section">
        <h3>Results</h3>

        <div class="metrics">
          <div class="pill">
            <p class="k">Total Protein</p>
            <p class="v"><span id="totalProtein">0</span> g</p>
          </div>
          <div class="pill">
            <p class="k">Total Fat</p>
            <p class="v"><span id="totalFat">0</span> g</p>
          </div>
          <div class="pill">
            <p class="k">Total Carbs</p>
            <p class="v"><span id="totalCarbs">0</span> g</p>
          </div>
        </div>

        <div class="metrics" style="margin-top:10px;">
          <div class="pill">
            <p class="k">Protein Balance</p>
            <p class="v"><span id="proteinBalance">0</span></p>
          </div>
          <div class="pill">
            <p class="k">Total Calories (Estimated)</p>
            <p class="v"><span id="totalCalories">0</span> kcal</p>
          </div>
          <div class="pill">
            <p class="k">Quick Action</p>
            <p class="v" style="margin:0;">
              <button onclick="saveReport()">Save Report as PDF</button>
            </p>
          </div>
        </div>

        <div class="chart-container">
          <h3 style="margin:0 0 10px; color:#0b2a5a;">Macro Breakdown</h3>
          <canvas id="macroChart"></canvas>
        </div>
      </div>

      <div class="section">
        <h3>TDEE and Target Calories</h3>

        <div class="row2">
          <div>
            <label for="activity_level">Activity Level</label>
            <select id="activity_level">
              <option value="1.2">Sedentary (little or no exercise)</option>
              <option value="1.375">Lightly Active (light exercise/sports 1-3 days/week)</option>
              <option value="1.55">Moderately Active (moderate exercise/sports 3-5 days/week)</option>
              <option value="1.725">Very Active (hard exercise/sports 6-7 days/week)</option>
              <option value="1.9">Extra Active (very hard exercise/sports & physical job)</option>
            </select>
          </div>
          <div>
            <label for="goal">Goal</label>
            <select id="goal">
              <option value="deficit">Deficit</option>
              <option value="maintenance">Maintenance</option>
            </select>
          </div>
        </div>

        <div style="margin-top:10px;">
          <button onclick="calculateTDEE()">Calculate TDEE and Target Calories</button>
        </div>

        <div class="metrics" style="margin-top:10px;">
          <div class="pill">
            <p class="k">TDEE</p>
            <p class="v"><span id="tdee">0</span> kcal</p>
          </div>
          <div class="pill">
            <p class="k">Target Calories/day</p>
            <p class="v"><span id="targetCalories">0</span> kcal</p>
          </div>
          <div class="pill">
            <p class="k">Note</p>
            <p class="v" style="font-size:12px; font-weight:600; color:#405071; margin-top:6px;">
              Deficit uses 1100 kcal reduction (as in your original code).
            </p>
          </div>
        </div>

      </div>

      <div class="disclaimer">
        <p><strong>Disclaimer:</strong> Values are estimates. For personalized dietary advice, consult a certified nutritionist/healthcare professional.</p>
      </div>
    </div>
  </div>

  <script>
    let macroChart;

    // ---------- Helpers ----------
    function num(id){
      const v = parseFloat(document.getElementById(id).value);
      return isNaN(v) ? 0 : v;
    }

    function toggleHeightInput() {
      const heightUnit = document.getElementById('height_unit').value;
      document.getElementById('height_cm_input').style.display = (heightUnit === 'cm') ? 'block' : 'none';
      document.getElementById('height_ft_in_input').style.display = (heightUnit === 'ft_in') ? 'block' : 'none';
    }

    function toggleWeightInput() {
      const weightUnit = document.getElementById('weight_unit').value;
      document.getElementById('weight_kg_input').style.display = (weightUnit === 'kg') ? 'block' : 'none';
      document.getElementById('weight_lbs_input').style.display = (weightUnit === 'lbs') ? 'block' : 'none';
      calculateRecommendedProtein();
    }

    function updateRequiredProtein() {
      document.getElementById('required_protein_display').innerText = num('required_protein').toFixed(0);
    }

    function calculateRecommendedProtein() {
      const weightUnit = document.getElementById('weight_unit').value;
      let weight = num(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs');
      if (weightUnit === 'lbs') weight = weight * 0.453592;

      if(weight <= 0){
        document.getElementById('recommended_protein').innerText = "0";
        return;
      }
      const proteinMin = (weight * 1.5).toFixed(0);
      const proteinMax = (weight * 2).toFixed(0);
      document.getElementById('recommended_protein').innerText = `${proteinMin} - ${proteinMax}`;
    }

    // ---------- BUTTER (USDA/FDC-derived) ----------
    // Using Salted Butter per 1 tbsp (14g): Calories 102, Fat 11.5g, Carbs 0.01g, Protein 0.12g
    // Source aligned to USDA/FoodData Central via MyFoodData.
    const BUTTER_PER_TBSP_G = 14.0;
    const BUTTER_TSP_G = BUTTER_PER_TBSP_G / 3.0; // 1 tbsp = 3 tsp (so ~4.6667g)
    const BUTTER_PER_TBSP = { cal: 102, fat: 11.5, carbs: 0.01, protein: 0.12 };

    function butterFromGrams(g){
      const factor = g / BUTTER_PER_TBSP_G;
      return {
        protein: BUTTER_PER_TBSP.protein * factor,
        fat: BUTTER_PER_TBSP.fat * factor,
        carbs: BUTTER_PER_TBSP.carbs * factor,
        calories: BUTTER_PER_TBSP.cal * factor
      };
    }

    function getButterGrams(){
      const unit = document.getElementById('butter_unit').value;
      const qty = num('butter_qty');
      if(qty <= 0) return 0;
      return (unit === 'g') ? qty : (qty * BUTTER_TSP_G);
    }

    function updateButterMacros(){
      const g = getButterGrams();
      const m = butterFromGrams(g);

      const line = document.getElementById('butter_macro_line');
      line.innerHTML = `
        <span>Protein: ${m.protein.toFixed(2)} g</span>
        <span>Fat: ${m.fat.toFixed(2)} g</span>
        <span>Carbs: ${m.carbs.toFixed(2)} g</span>
        <span>Calories: ${m.calories.toFixed(2)} kcal</span>
      `;
    }

    // ---------- Main Macro Calculation ----------
    function calculateMacros() {
      updateButterMacros();

      const requiredProtein = num('required_protein');

      // Weight & Height for BMI
      const weightUnit = document.getElementById('weight_unit').value;
      let weight = num(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs');
      if (weightUnit === 'lbs') weight *= 0.453592;

      const heightUnit = document.getElementById('height_unit').value;
      let heightM = 0;
      if (heightUnit === 'cm') {
        heightM = num('height_cm') / 100;
      } else {
        const feet = num('height_ft');
        const inches = num('height_in');
        heightM = (feet * 0.3048) + (inches * 0.0254);
      }

      const bmi = (weight > 0 && heightM > 0) ? (weight / (heightM * heightM)) : 0;
      document.getElementById('bmi').innerText = bmi.toFixed(2);

      // Protein (your original assumptions)
      const chicken_breast = num('chicken_breast') * 0.31;
      const chicken_thigh  = num('chicken_thigh')  * 0.26;
      const chicken_wings  = num('chicken_wings')  * 0.30;
      const chicken_drum   = num('chicken_drumsticks') * 0.28;
      const mutton         = num('mutton') * 0.25;
      const duck           = num('duck') * 0.23;
      const pigeon         = num('pigeon') * 0.25;
      const quail          = num('quail') * 0.22;
      const beef           = num('beef') * 0.26;
      const fish           = num('fish') * 0.22;
      const shrimp         = num('shrimp') * 0.24;
      const dried_fish     = num('dried_fish') * 0.60;
      const egg            = num('egg') * 6;
      const protein_shake  = num('protein_shake') * 25;
      const riceProtein    = num('rice') * 4;
      const breadProtein   = num('bread') * 3;
      const saladProtein   = num('salad') * 0.5;
      const vegProtein     = num('vegetables') * 2;
      const chocProtein    = num('chocolate') * 1;
      const cakeProtein    = num('cake') * 3;

      // Butter macros
      const butterG = getButterGrams();
      const butterM = butterFromGrams(butterG);

      // Fat (your original assumptions)
      const fatFromChickenThigh = num('chicken_thigh') * 0.08;
      const fatFromChickenDrum  = num('chicken_drumsticks') * 0.10;
      const fatFromMutton       = num('mutton') * 0.20;
      const fatFromDuck         = num('duck') * 0.28;
      const fatFromPigeon       = num('pigeon') * 0.15;
      const fatFromQuail        = num('quail') * 0.12;
      const fatFromBeef         = num('beef') * 0.15;
      const fatFromFish         = num('fish') * 0.05;
      const fatFromShrimp       = num('shrimp') * 0.02;
      const fatFromDriedFish    = num('dried_fish') * 0.05;
      const fatFromOil          = num('oil') * 14;
      const fatFromChocolate    = num('chocolate') * 5;
      const fatFromCake         = num('cake') * 10;

      // Carbs (your original assumptions)
      const carbsFromRice        = num('rice') * 45;
      const carbsFromBread       = num('bread') * 15;
      const carbsFromSalad       = num('salad') * 2;
      const carbsFromVegetables  = num('vegetables') * 5;
      const carbsFromMilkTea     = num('milk_tea') * 10;
      const carbsFromChocolate   = num('chocolate') * 15;
      const carbsFromCake        = num('cake') * 30;
      const carbsFromSoda        = num('carbonated_beverage') * 40;

      // Total macros (+ butter)
      const totalProtein =
        chicken_breast + chicken_thigh + chicken_wings + chicken_drum +
        mutton + duck + pigeon + quail + beef + fish + shrimp + dried_fish +
        egg + protein_shake + riceProtein + breadProtein + saladProtein + vegProtein +
        chocProtein + cakeProtein + butterM.protein;

      const totalFat =
        fatFromChickenThigh + fatFromChickenDrum + fatFromMutton + fatFromDuck +
        fatFromPigeon + fatFromQuail + fatFromBeef + fatFromFish + fatFromShrimp +
        fatFromDriedFish + fatFromOil + fatFromChocolate + fatFromCake +
        butterM.fat;

      const totalCarbs =
        carbsFromRice + carbsFromBread + carbsFromSalad + carbsFromVegetables +
        carbsFromMilkTea + carbsFromChocolate + carbsFromCake + carbsFromSoda +
        butterM.carbs;

      const proteinBalance = requiredProtein - totalProtein;

      // Calories:
      // Use macro-based calories (your original method) + also butter is already included via fat/protein/carbs,
      // so no need to add butter calories separately.
      const totalCalories = (totalProtein * 4) + (totalCarbs * 4) + (totalFat * 9);

      // Update display
      document.getElementById('totalProtein').innerText = totalProtein.toFixed(2);
      document.getElementById('totalFat').innerText = totalFat.toFixed(2);
      document.getElementById('totalCarbs').innerText = totalCarbs.toFixed(2);

      const pb = document.getElementById('proteinBalance');
      pb.innerText = proteinBalance.toFixed(2) + " g";
      pb.className = (proteinBalance > 0) ? "danger" : "ok";

      document.getElementById('totalCalories').innerText = totalCalories.toFixed(2);

      updateChart(totalProtein, totalFat, totalCarbs);
    }

    function calculateTDEE() {
      const weightUnit = document.getElementById('weight_unit').value;
      let weight = num(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs');
      if (weightUnit === 'lbs') weight *= 0.453592;

      const heightUnit = document.getElementById('height_unit').value;
      let height = 0;
      if (heightUnit === 'cm') {
        height = num('height_cm') / 100;
      } else {
        const feet = num('height_ft');
        const inches = num('height_in');
        height = (feet * 0.3048) + (inches * 0.0254);
      }

      const age = num('age');
      const activityLevel = parseFloat(document.getElementById('activity_level').value);
      const goal = document.getElementById('goal').value;

      // BMR (as in your original code: male)
      const bmr = (10 * weight) + (6.25 * height * 100) - (5 * age) + 5;
      const tdee = bmr * activityLevel;
      const targetCalories = (goal === "deficit") ? (tdee - 1100) : tdee;

      document.getElementById('tdee').innerText = tdee.toFixed(2);
      document.getElementById('targetCalories').innerText = targetCalories.toFixed(2);
    }

    function updateChart(protein, fat, carbs) {
      const ctx = document.getElementById('macroChart').getContext('2d');
      if (macroChart) macroChart.destroy();

      macroChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ['Protein', 'Fat', 'Carbohydrates'],
          datasets: [{
            label: 'Macro Breakdown (g)',
            data: [protein, fat, carbs],
            backgroundColor: ['#2f7cff', '#ffb703', '#00c2ff'],
            borderRadius: 10
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: { display: true }
          },
          scales: {
            y: { beginAtZero: true }
          }
        }
      });
    }

    function saveReport() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();

      doc.setFontSize(18);
      doc.text("Nowshad's Macro Calculator Report", 10, 12);

      const date = new Date().toLocaleString();
      doc.setFontSize(11);
      doc.text(`Report Generated on: ${date}`, 10, 20);

      doc.setFontSize(13);
      doc.text("User Details", 10, 30);

      doc.setFontSize(11);
      doc.text(`Name: ${document.getElementById('name').value}`, 10, 38);
      doc.text(`Age: ${document.getElementById('age').value}`, 10, 45);
      doc.text(`BMI: ${document.getElementById('bmi').innerText}`, 10, 52);

      doc.text(`Recommended Protein Intake (g): ${document.getElementById('recommended_protein').innerText}`, 10, 60);
      doc.text(`Required Protein (g): ${document.getElementById('required_protein_display').innerText}`, 10, 67);

      doc.setFontSize(13);
      doc.text("Macro Totals", 10, 78);
      doc.setFontSize(11);
      doc.text(`Total Protein (g): ${document.getElementById('totalProtein').innerText}`, 10, 86);
      doc.text(`Total Fat (g): ${document.getElementById('totalFat').innerText}`, 10, 93);
      doc.text(`Total Carbs (g): ${document.getElementById('totalCarbs').innerText}`, 10, 100);
      doc.text(`Protein Balance: ${document.getElementById('proteinBalance').innerText}`, 10, 107);
      doc.text(`Total Calories (kcal): ${document.getElementById('totalCalories').innerText}`, 10, 114);

      doc.setFontSize(13);
      doc.text("TDEE and Target Calories", 10, 125);
      doc.setFontSize(11);
      doc.text(`TDEE: ${document.getElementById('tdee').innerText} kcal`, 10, 133);
      doc.text(`Target Calories per Day: ${document.getElementById('targetCalories').innerText} kcal`, 10, 140);

      doc.setFontSize(10);
      doc.text(
        "Disclaimer: Values are estimates. For personalized dietary advice, consult a certified nutritionist/healthcare professional.",
        10, 155, { maxWidth: 190 }
      );

      doc.save("Nowshad_Macro_Calculator_Report.pdf");
    }

    // Initialize butter line once
    updateButterMacros();
  </script>
</body>
</html>
