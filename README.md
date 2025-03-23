Nowshad's Macro calculator 
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nowshad's Macro Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f9;
            color: #333;
        }
        .container {
            max-width: 500px;
            margin: auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        h2 {
            color: #4CAF50;
            text-align: center;
        }
        label {
            font-weight: bold;
            color: #555;
        }
        input, button, select {
            margin: 10px 0;
            padding: 10px;
            width: 100%;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #45a049;
        }
        h3 {
            color: #4CAF50;
        }
        .disclaimer {
            font-size: 12px;
            color: #666;
            margin-top: 20px;
            text-align: center;
        }
        .required-protein {
            color: #e74c3c;
            font-weight: bold;
        }
        .highlight-red {
            color: red;
        }
        .chart-container {
            margin-top: 20px;
            text-align: center;
        }
        .chart {
            width: 100%;
            max-width: 400px;
            margin: auto;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <div class="container">
        <h2>Nowshad's Macro Calculator </h2>
        
        <label for="name">Name:</label>
        <input type="text" id="name" placeholder="Enter your name">
        
        <label for="age">Age:</label>
        <input type="number" id="age" placeholder="Enter your age">
        
        <!-- Height Input -->
        <label for="height_unit">Height Unit:</label>
        <select id="height_unit" onchange="toggleHeightInput()">
            <option value="cm">Centimeters (cm)</option>
            <option value="ft_in">Feet/Inches (ft/in)</option>
        </select>
        
        <div id="height_cm_input">
            <label for="height_cm">Height (cm):</label>
            <input type="number" id="height_cm" placeholder="Enter your height in cm">
        </div>
        
        <div id="height_ft_in_input" style="display: none;">
            <label for="height_ft">Height (ft):</label>
            <input type="number" id="height_ft" placeholder="Feet">
            <label for="height_in">Height (in):</label>
            <input type="number" id="height_in" placeholder="Inches">
        </div>
        
        <!-- Weight Input -->
        <label for="weight_unit">Weight Unit:</label>
        <select id="weight_unit" onchange="toggleWeightInput()">
            <option value="kg">Kilograms (kg)</option>
            <option value="lbs">Pounds (lbs)</option>
        </select>
        
        <div id="weight_kg_input">
            <label for="weight_kg">Weight (kg):</label>
            <input type="number" id="weight_kg" placeholder="Enter your weight in kg" oninput="calculateRecommendedProtein()">
        </div>
        
        <div id="weight_lbs_input" style="display: none;">
            <label for="weight_lbs">Weight (lbs):</label>
            <input type="number" id="weight_lbs" placeholder="Enter your weight in lbs" oninput="calculateRecommendedProtein()">
        </div>
        
        <h3>BMI: <span id="bmi">0</span></h3>
        
        <h3>Recommended Protein Intake: <span class="required-protein" id="recommended_protein">0</span> g</h3>
        
        <h3 style="color: red;">Required Protein: <span id="required_protein_display">0</span> g</h3>
        <label for="required_protein">Enter Required Protein (grams):</label>
        <input type="number" id="required_protein" placeholder="Enter required protein" oninput="updateRequiredProtein()">
        
        <!-- Protein Shake and Eggs -->
        <label for="protein_shake">Protein Shake (scoops):</label>
        <input type="number" id="protein_shake" placeholder="Enter scoops">
        
        <label for="egg">Eggs (count):</label>
        <input type="number" id="egg" placeholder="Enter count">
        
        <!-- Chicken Items -->
        <label for="chicken_breast">Chicken Breast (grams):</label>
        <input type="number" id="chicken_breast" placeholder="Enter grams">
        
        <label for="chicken_thigh">Chicken Thigh (grams):</label>
        <input type="number" id="chicken_thigh" placeholder="Enter grams">
        
        <label for="chicken_wings">Chicken Wings (grams):</label>
        <input type="number" id="chicken_wings" placeholder="Enter grams">
        
        <label for="chicken_drumsticks">Chicken Drumsticks (grams):</label>
        <input type="number" id="chicken_drumsticks" placeholder="Enter grams">
        
        <!-- Other Meats -->
        <label for="mutton">Mutton (grams):</label>
        <input type="number" id="mutton" placeholder="Enter grams">
        
        <label for="duck">Duck (grams):</label>
        <input type="number" id="duck" placeholder="Enter grams">
        
        <label for="pigeon">Pigeon (grams):</label>
        <input type="number" id="pigeon" placeholder="Enter grams">
        
        <label for="quail">Quail (grams):</label>
        <input type="number" id="quail" placeholder="Enter grams">
        
        <label for="beef">Beef (grams):</label>
        <input type="number" id="beef" placeholder="Enter grams">
        
        <label for="fish">Fish (grams):</label>
        <input type="number" id="fish" placeholder="Enter grams">
        
        <label for="shrimp">Shrimp (grams):</label>
        <input type="number" id="shrimp" placeholder="Enter grams">
        
        <label for="dried_fish">Dried Fish/Dried Bombay Duck (grams):</label>
        <input type="number" id="dried_fish" placeholder="Enter grams">
        
        <!-- Other Items -->
        <label for="rice">Rice (cups):</label>
        <input type="number" id="rice" placeholder="Enter cups">
        
        <label for="bread">Bread (slices):</label>
        <input type="number" id="bread" placeholder="Enter slices">
        
        <label for="oil">Oil (tablespoons):</label>
        <input type="number" id="oil" placeholder="Enter tablespoons">
        
        <label for="salad">Salad (cups):</label>
        <input type="number" id="salad" placeholder="Enter cups">
        
        <label for="vegetables">Vegetables (cups):</label>
        <input type="number" id="vegetables" placeholder="Enter cups">
        
        <!-- New Food Items -->
        <label for="milk_tea">Milk Tea (cups):</label>
        <input type="number" id="milk_tea" placeholder="Enter cups">
        
        <label for="chocolate">Cube of Chocolate (count):</label>
        <input type="number" id="chocolate" placeholder="Enter count">
        
        <label for="cake">Slice of Cake (count):</label>
        <input type="number" id="cake" placeholder="Enter count">
        
        <label for="carbonated_beverage">Carbonated Beverage (cans):</label>
        <input type="number" id="carbonated_beverage" placeholder="Enter cans">
        
        <button onclick="calculateMacros()">Calculate Macros</button>
        
        <h3>Total Protein Intake: <span id="totalProtein">0</span> g</h3>
        <h3>Total Fat Intake: <span id="totalFat">0</span> g</h3>
        <h3>Total Carbohydrate Intake: <span id="totalCarbs">0</span> g</h3>
        <h3>Protein Balance: <span id="proteinBalance">0</span> g</h3>
        <h3>Total Calorie Intake (Estimated): <span id="totalCalories">0</span> kcal</h3>

        <!-- TDEE and Target Calories -->
        <div>
            <h3>TDEE and Target Calories</h3>
            <label for="activity_level">Select Activity Level:</label>
            <select id="activity_level">
                <option value="1.2">Sedentary (little or no exercise)</option>
                <option value="1.375">Lightly Active (light exercise/sports 1-3 days/week)</option>
                <option value="1.55">Moderately Active (moderate exercise/sports 3-5 days/week)</option>
                <option value="1.725">Very Active (hard exercise/sports 6-7 days/week)</option>
                <option value="1.9">Extra Active (very hard exercise/sports & physical job)</option>
            </select>
            
            <label for="goal">Select Goal:</label>
            <select id="goal">
                <option value="deficit">Deficit</option>
                <option value="maintenance">Maintenance</option>
            </select>
            
            <button onclick="calculateTDEE()">Calculate TDEE and Target Calories</button>
            
            <h3>TDEE (Total Daily Energy Expenditure): <span id="tdee">0</span> kcal</h3>
            <h3>Target Calories per Day: <span id="targetCalories">0</span> kcal</h3>
        </div>

        <!-- Graphical Representation -->
        <div class="chart-container">
            <h3>Macro Breakdown</h3>
            <canvas id="macroChart" class="chart"></canvas>
        </div>

        <!-- Save Report Button -->
        <button onclick="saveReport()">Save Report as PDF</button>

        <!-- Disclaimer Section -->
        <div class="disclaimer">
            <p><strong>Disclaimer:</strong> The values provided by this calculator are estimates based on available reliable sources. For accurate and personalized dietary advice, please consult a certified nutritionist or healthcare professional before using this information as a reference.</p>
        </div>
    </div>

    <script>
        let macroChart;

        function toggleHeightInput() {
            const heightUnit = document.getElementById('height_unit').value;
            if (heightUnit === 'cm') {
                document.getElementById('height_cm_input').style.display = 'block';
                document.getElementById('height_ft_in_input').style.display = 'none';
            } else {
                document.getElementById('height_cm_input').style.display = 'none';
                document.getElementById('height_ft_in_input').style.display = 'block';
            }
        }

        function toggleWeightInput() {
            const weightUnit = document.getElementById('weight_unit').value;
            if (weightUnit === 'kg') {
                document.getElementById('weight_kg_input').style.display = 'block';
                document.getElementById('weight_lbs_input').style.display = 'none';
            } else {
                document.getElementById('weight_kg_input').style.display = 'none';
                document.getElementById('weight_lbs_input').style.display = 'block';
            }
        }

        function updateRequiredProtein() {
            document.getElementById('required_protein_display').innerText = document.getElementById('required_protein').value;
        }

        function calculateRecommendedProtein() {
            const weightUnit = document.getElementById('weight_unit').value;
            let weight = parseFloat(document.getElementById(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs').value);
            if (weightUnit === 'lbs') {
                weight = weight * 0.453592; // Convert lbs to kg
            }
            const proteinMin = (weight * 1.5).toFixed(2);
            const proteinMax = (weight * 2).toFixed(2);
            document.getElementById('recommended_protein').innerText = `${proteinMin} - ${proteinMax} g`;
        }

        function calculateMacros() {
            const requiredProtein = parseFloat(document.getElementById('required_protein').value);
            const weightUnit = document.getElementById('weight_unit').value;
            let weight = parseFloat(document.getElementById(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs').value);
            if (weightUnit === 'lbs') {
                weight = weight * 0.453592; // Convert lbs to kg
            }
            const heightUnit = document.getElementById('height_unit').value;
            let height = 0;
            if (heightUnit === 'cm') {
                height = parseFloat(document.getElementById('height_cm').value) / 100; // Convert cm to meters
            } else {
                const feet = parseFloat(document.getElementById('height_ft').value);
                const inches = parseFloat(document.getElementById('height_in').value);
                height = (feet * 0.3048) + (inches * 0.0254); // Convert ft/in to meters
            }
            
            const bmi = (weight / (height * height)).toFixed(2);
            document.getElementById('bmi').innerText = bmi;
            
            // Protein calculations
            const chicken_breast = document.getElementById('chicken_breast').value * 0.31; // 31g protein per 100g
            const chicken_thigh = document.getElementById('chicken_thigh').value * 0.26; // 26g protein per 100g
            const chicken_wings = document.getElementById('chicken_wings').value * 0.30; // 30g protein per 100g
            const chicken_drumsticks = document.getElementById('chicken_drumsticks').value * 0.28; // 28g protein per 100g
            const mutton = document.getElementById('mutton').value * 0.25; // 25g protein per 100g
            const duck = document.getElementById('duck').value * 0.23; // 23g protein per 100g
            const pigeon = document.getElementById('pigeon').value * 0.25; // 25g protein per 100g
            const quail = document.getElementById('quail').value * 0.22; // 22g protein per 100g
            const beef = document.getElementById('beef').value * 0.26; // 26g protein per 100g
            const fish = document.getElementById('fish').value * 0.22; // 22g protein per 100g
            const shrimp = document.getElementById('shrimp').value * 0.24; // 24g protein per 100g
            const dried_fish = document.getElementById('dried_fish').value * 0.60; // 60g protein per 100g
            const egg = document.getElementById('egg').value * 6; // 6g protein per egg
            const protein_shake = document.getElementById('protein_shake').value * 25; // 25g protein per scoop
            const rice = document.getElementById('rice').value * 4; // 4g protein per cup
            const bread = document.getElementById('bread').value * 3; // 3g protein per slice
            const oil = document.getElementById('oil').value * 0; // 0g protein per tablespoon
            const salad = document.getElementById('salad').value * 0.5; // 0.5g protein per cup
            const vegetables = document.getElementById('vegetables').value * 2; // 2g protein per cup
            const milk_tea = document.getElementById('milk_tea').value * 0; // 0g protein per cup
            const chocolate = document.getElementById('chocolate').value * 1; // 1g protein per cube
            const cake = document.getElementById('cake').value * 3; // 3g protein per slice
            const carbonated_beverage = document.getElementById('carbonated_beverage').value * 0; // 0g protein per can
            
            // Fat calculations
            const fatFromChickenThigh = document.getElementById('chicken_thigh').value * 0.08; // 8g fat per 100g
            const fatFromChickenDrumsticks = document.getElementById('chicken_drumsticks').value * 0.10; // 10g fat per 100g
            const fatFromMutton = document.getElementById('mutton').value * 0.20; // 20g fat per 100g
            const fatFromDuck = document.getElementById('duck').value * 0.28; // 28g fat per 100g
            const fatFromPigeon = document.getElementById('pigeon').value * 0.15; // 15g fat per 100g
            const fatFromQuail = document.getElementById('quail').value * 0.12; // 12g fat per 100g
            const fatFromBeef = document.getElementById('beef').value * 0.15; // 15g fat per 100g
            const fatFromFish = document.getElementById('fish').value * 0.05; // 5g fat per 100g
            const fatFromShrimp = document.getElementById('shrimp').value * 0.02; // 2g fat per 100g
            const fatFromDriedFish = document.getElementById('dried_fish').value * 0.05; // 5g fat per 100g
            const fatFromOil = document.getElementById('oil').value * 14; // 14g fat per tablespoon
            const fatFromMilkTea = document.getElementById('milk_tea').value * 0; // 0g fat per cup
            const fatFromChocolate = document.getElementById('chocolate').value * 5; // 5g fat per cube
            const fatFromCake = document.getElementById('cake').value * 10; // 10g fat per slice
            const fatFromCarbonatedBeverage = document.getElementById('carbonated_beverage').value * 0; // 0g fat per can
            
            // Carb calculations
            const carbsFromRice = document.getElementById('rice').value * 45; // 45g carbs per cup
            const carbsFromBread = document.getElementById('bread').value * 15; // 15g carbs per slice
            const carbsFromSalad = document.getElementById('salad').value * 2; // 2g carbs per cup
            const carbsFromVegetables = document.getElementById('vegetables').value * 5; // 5g carbs per cup
            const carbsFromMilkTea = document.getElementById('milk_tea').value * 10; // 10g carbs per cup
            const carbsFromChocolate = document.getElementById('chocolate').value * 15; // 15g carbs per cube
            const carbsFromCake = document.getElementById('cake').value * 30; // 30g carbs per slice
            const carbsFromCarbonatedBeverage = document.getElementById('carbonated_beverage').value * 40; // 40g carbs per can
            
            // Total macros
            const totalProtein = chicken_breast + chicken_thigh + chicken_wings + chicken_drumsticks + mutton + duck + pigeon + quail + beef + fish + shrimp + dried_fish + egg + protein_shake + rice + bread + oil + salad + vegetables + milk_tea + chocolate + cake + carbonated_beverage;
            const totalFat = fatFromChickenThigh + fatFromChickenDrumsticks + fatFromMutton + fatFromDuck + fatFromPigeon + fatFromQuail + fatFromBeef + fatFromFish + fatFromShrimp + fatFromDriedFish + fatFromOil + fatFromMilkTea + fatFromChocolate + fatFromCake + fatFromCarbonatedBeverage;
            const totalCarbs = carbsFromRice + carbsFromBread + carbsFromSalad + carbsFromVegetables + carbsFromMilkTea + carbsFromChocolate + carbsFromCake + carbsFromCarbonatedBeverage;
            const proteinBalance = requiredProtein - totalProtein;
            
            // Calorie calculation
            const totalCalories = (totalProtein * 4) + (totalCarbs * 4) + (totalFat * 9); // 4 kcal/g for protein and carbs, 9 kcal/g for fat
            
            // Update the display
            document.getElementById('totalProtein').innerText = totalProtein.toFixed(2);
            document.getElementById('totalFat').innerText = totalFat.toFixed(2);
            document.getElementById('totalCarbs').innerText = totalCarbs.toFixed(2);
            document.getElementById('proteinBalance').innerText = proteinBalance.toFixed(2) + " g";
            document.getElementById('proteinBalance').className = proteinBalance > 0 ? "highlight-red" : "";
            document.getElementById('totalCalories').innerText = totalCalories.toFixed(2);

            // Update the chart
            updateChart(totalProtein, totalFat, totalCarbs);
        }

        function calculateTDEE() {
            const weightUnit = document.getElementById('weight_unit').value;
            let weight = parseFloat(document.getElementById(weightUnit === 'kg' ? 'weight_kg' : 'weight_lbs').value);
            if (weightUnit === 'lbs') {
                weight = weight * 0.453592; // Convert lbs to kg
            }
            const heightUnit = document.getElementById('height_unit').value;
            let height = 0;
            if (heightUnit === 'cm') {
                height = parseFloat(document.getElementById('height_cm').value) / 100; // Convert cm to meters
            } else {
                const feet = parseFloat(document.getElementById('height_ft').value);
                const inches = parseFloat(document.getElementById('height_in').value);
                height = (feet * 0.3048) + (inches * 0.0254); // Convert ft/in to meters
            }
            const age = parseFloat(document.getElementById('age').value);
            const activityLevel = parseFloat(document.getElementById('activity_level').value);
            const goal = document.getElementById('goal').value;

            // BMR calculation using Mifflin-St Jeor formula
            const bmr = (10 * weight) + (6.25 * height * 100) - (5 * age) + 5; // For men
            const tdee = bmr * activityLevel;
            const targetCalories = goal === "deficit" ? tdee - 1100 : tdee; // 1100 kcal deficit for weight loss

            document.getElementById('tdee').innerText = tdee.toFixed(2);
            document.getElementById('targetCalories').innerText = targetCalories.toFixed(2);
        }

        function updateChart(protein, fat, carbs) {
            const ctx = document.getElementById('macroChart').getContext('2d');
            if (macroChart) {
                macroChart.destroy();
            }
            macroChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Protein', 'Fat', 'Carbohydrates'],
                    datasets: [{
                        label: 'Macro Breakdown (g)',
                        data: [protein, fat, carbs],
                        backgroundColor: ['#4CAF50', '#FFC107', '#2196F3'],
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
        }

        function saveReport() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            // Report Heading
            doc.setFontSize(18);
            doc.text("Nowshad's Macro Calculator Report", 10, 10);

            // Report Generation Date and Time
            const date = new Date().toLocaleString();
            doc.setFontSize(12);
            doc.text(`Report Generated on: ${date}`, 10, 20);

            // User Details
            doc.setFontSize(14);
            doc.text("User Details", 10, 30);
            doc.setFontSize(12);
            doc.text(`Name: ${document.getElementById('name').value}`, 10, 40);
            doc.text(`Age: ${document.getElementById('age').value}`, 10, 50);
            doc.text(`Height: ${document.getElementById('height_unit').value === 'cm' ? document.getElementById('height_cm').value + ' cm' : document.getElementById('height_ft').value + ' ft ' + document.getElementById('height_in').value + ' in'}`, 10, 60);
            doc.text(`Weight: ${document.getElementById('weight_unit').value === 'kg' ? document.getElementById('weight_kg').value + ' kg' : document.getElementById('weight_lbs').value + ' lbs'}`, 10, 70);
            doc.text(`BMI: ${document.getElementById('bmi').innerText}`, 10, 80);

            // Protein Intake
            doc.setFontSize(14);
            doc.text("Protein Intake", 10, 90);
            doc.setFontSize(12);
            doc.text(`Recommended Protein Intake: ${document.getElementById('recommended_protein').innerText}`, 10, 100);
            doc.text(`Required Protein: ${document.getElementById('required_protein_display').innerText} g`, 10, 110);
            doc.text(`Total Protein Intake: ${document.getElementById('totalProtein').innerText} g`, 10, 120);
            doc.text(`Protein Balance: ${document.getElementById('proteinBalance').innerText}`, 10, 130);

            // Fat and Carbohydrate Intake
            doc.setFontSize(14);
            doc.text("Fat and Carbohydrate Intake", 10, 140);
            doc.setFontSize(12);
            doc.text(`Total Fat Intake: ${document.getElementById('totalFat').innerText} g`, 10, 150);
            doc.text(`Total Carbohydrate Intake: ${document.getElementById('totalCarbs').innerText} g`, 10, 160);

            // Calorie Intake
            doc.setFontSize(14);
            doc.text("Calorie Intake", 10, 170);
            doc.setFontSize(12);
            doc.text(`Total Calorie Intake: ${document.getElementById('totalCalories').innerText} kcal`, 10, 180);

            // TDEE and Target Calories
            doc.setFontSize(14);
            doc.text("TDEE and Target Calories", 10, 190);
            doc.setFontSize(12);
            doc.text(`TDEE: ${document.getElementById('tdee').innerText} kcal`, 10, 200);
            doc.text(`Target Calories per Day: ${document.getElementById('targetCalories').innerText} kcal`, 10, 210);

            // Disclaimer
            doc.setFontSize(10);
            doc.text("Disclaimer: The values provided by this calculator are estimates based on available reliable sources. For accurate and personalized dietary advice, please consult a certified nutritionist or healthcare professional before using this information as a reference.", 10, 220, { maxWidth: 180 });

            // Save the PDF
            doc.save("Nowshad_Macro_Calculator_Report.pdf");
        }
    </script>
</body>
</html>
