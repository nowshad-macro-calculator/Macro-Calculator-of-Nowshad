Nowshad's Macro Calculator
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
    </style>
</head>
<body>
    <div class="container">
        <h2>Nowshad's Macro Calculator (Inspired by ST's Nutrition Plan)</h2>
        
        <label for="name">Name:</label>
        <input type="text" id="name" placeholder="Enter your name">
        
        <label for="age">Age:</label>
        <input type="number" id="age" placeholder="Enter your age">
        
        <label for="height">Height (cm):</label>
        <input type="number" id="height" placeholder="Enter your height">
        
        <label for="weight">Weight (kg):</label>
        <input type="number" id="weight" placeholder="Enter your weight" oninput="calculateRecommendedProtein()">
        
        <h3>BMI: <span id="bmi">0</span></h3>
        
        <h3>Recommended Protein Intake: <span class="required-protein" id="recommended_protein">0</span> g</h3>
        
        <h3 style="color: red;">Required Protein: <span id="required_protein_display">0</span> g</h3>
        <label for="required_protein">Enter Required Protein (grams):</label>
        <input type="number" id="required_protein" value="0" oninput="updateRequiredProtein()">
        
        <!-- Protein Shake and Eggs -->
        <label for="protein_shake">Protein Shake (scoops):</label>
        <input type="number" id="protein_shake" value="0">
        
        <label for="egg">Eggs (count):</label>
        <input type="number" id="egg" value="0">
        
        <!-- Chicken Items -->
        <label for="chicken_breast">Chicken Breast (grams):</label>
        <input type="number" id="chicken_breast" value="0">
        
        <label for="chicken_thigh">Chicken Thigh (grams):</label>
        <input type="number" id="chicken_thigh" value="0">
        
        <label for="chicken_wings">Chicken Wings (grams):</label>
        <input type="number" id="chicken_wings" value="0">
        
        <label for="chicken_drumsticks">Chicken Drumsticks (grams):</label>
        <input type="number" id="chicken_drumsticks" value="0">
        
        <!-- Other Meats -->
        <label for="mutton">Mutton (grams):</label>
        <input type="number" id="mutton" value="0">
        
        <label for="duck">Duck (grams):</label>
        <input type="number" id="duck" value="0">
        
        <label for="pigeon">Pigeon (grams):</label>
        <input type="number" id="pigeon" value="0">
        
        <label for="quail">Quail (grams):</label>
        <input type="number" id="quail" value="0">
        
        <label for="beef">Beef (grams):</label>
        <input type="number" id="beef" value="0">
        
        <label for="fish">Fish (grams):</label>
        <input type="number" id="fish" value="0">
        
        <label for="shrimp">Shrimp (grams):</label>
        <input type="number" id="shrimp" value="0">
        
        <label for="dried_fish">Dried Fish/Dried Bombay Duck (grams):</label>
        <input type="number" id="dried_fish" value="0">
        
        <!-- Other Items -->
        <label for="rice">Rice (cups):</label>
        <input type="number" id="rice" value="0">
        
        <label for="bread">Bread (slices):</label>
        <input type="number" id="bread" value="0">
        
        <label for="oil">Oil (tablespoons):</label>
        <input type="number" id="oil" value="0">
        
        <label for="salad">Salad (cups):</label>
        <input type="number" id="salad" value="0">
        
        <label for="vegetables">Vegetables (cups):</label>
        <input type="number" id="vegetables" value="0">
        
        <!-- New Food Items -->
        <label for="milk_tea">Milk Tea (cups):</label>
        <input type="number" id="milk_tea" value="0">
        
        <label for="chocolate">Cube of Chocolate (count):</label>
        <input type="number" id="chocolate" value="0">
        
        <label for="cake">Slice of Cake (count):</label>
        <input type="number" id="cake" value="0">
        
        <label for="carbonated_beverage">Carbonated Beverage (cans):</label>
        <input type="number" id="carbonated_beverage" value="0">
        
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

        <!-- Disclaimer Section -->
        <div class="disclaimer">
            <p><strong>Disclaimer:</strong> The values provided by this calculator are estimates based on available reliable sources. For accurate and personalized dietary advice, please consult a certified nutritionist or healthcare professional before using this information as a reference.</p>
        </div>
    </div>

    <script>
        function updateRequiredProtein() {
            document.getElementById('required_protein_display').innerText = document.getElementById('required_protein').value;
        }

        function calculateRecommendedProtein() {
            let weight = document.getElementById('weight').value;
            let recommendedProtein = weight * 2; // 2g protein per kg of body weight
            document.getElementById('recommended_protein').innerText = recommendedProtein.toFixed(2);
        }

        function calculateMacros() {
            let requiredProtein = document.getElementById('required_protein').value;
            let weight = document.getElementById('weight').value;
            let height = document.getElementById('height').value / 100;
            
            let bmi = (weight / (height * height)).toFixed(2);
            document.getElementById('bmi').innerText = bmi;
            
            // Protein calculations
            let chicken_breast = document.getElementById('chicken_breast').value * 0.31; // 31g protein per 100g
            let chicken_thigh = document.getElementById('chicken_thigh').value * 0.26; // 26g protein per 100g
            let chicken_wings = document.getElementById('chicken_wings').value * 0.30; // 30g protein per 100g
            let chicken_drumsticks = document.getElementById('chicken_drumsticks').value * 0.28; // 28g protein per 100g
            let mutton = document.getElementById('mutton').value * 0.25; // 25g protein per 100g
            let duck = document.getElementById('duck').value * 0.23; // 23g protein per 100g
            let pigeon = document.getElementById('pigeon').value * 0.25; // 25g protein per 100g
            let quail = document.getElementById('quail').value * 0.22; // 22g protein per 100g
            let beef = document.getElementById('beef').value * 0.26; // 26g protein per 100g
            let fish = document.getElementById('fish').value * 0.22; // 22g protein per 100g
            let shrimp = document.getElementById('shrimp').value * 0.24; // 24g protein per 100g
            let dried_fish = document.getElementById('dried_fish').value * 0.60; // 60g protein per 100g
            let egg = document.getElementById('egg').value * 6; // 6g protein per egg
            let protein_shake = document.getElementById('protein_shake').value * 25; // 25g protein per scoop
            let rice = document.getElementById('rice').value * 4; // 4g protein per cup
            let bread = document.getElementById('bread').value * 3; // 3g protein per slice
            let oil = document.getElementById('oil').value * 0; // 0g protein per tablespoon
            let salad = document.getElementById('salad').value * 0.5; // 0.5g protein per cup
            let vegetables = document.getElementById('vegetables').value * 2; // 2g protein per cup
            let milk_tea = document.getElementById('milk_tea').value * 0; // 0g protein per cup
            let chocolate = document.getElementById('chocolate').value * 1; // 1g protein per cube
            let cake = document.getElementById('cake').value * 3; // 3g protein per slice
            let carbonated_beverage = document.getElementById('carbonated_beverage').value * 0; // 0g protein per can
            
            // Fat calculations
            let fatFromChickenThigh = document.getElementById('chicken_thigh').value * 0.08; // 8g fat per 100g
            let fatFromChickenDrumsticks = document.getElementById('chicken_drumsticks').value * 0.10; // 10g fat per 100g
            let fatFromMutton = document.getElementById('mutton').value * 0.20; // 20g fat per 100g
            let fatFromDuck = document.getElementById('duck').value * 0.28; // 28g fat per 100g
            let fatFromPigeon = document.getElementById('pigeon').value * 0.15; // 15g fat per 100g
            let fatFromQuail = document.getElementById('quail').value * 0.12; // 12g fat per 100g
            let fatFromBeef = document.getElementById('beef').value * 0.15; // 15g fat per 100g
            let fatFromFish = document.getElementById('fish').value * 0.05; // 5g fat per 100g
            let fatFromShrimp = document.getElementById('shrimp').value * 0.02; // 2g fat per 100g
            let fatFromDriedFish = document.getElementById('dried_fish').value * 0.05; // 5g fat per 100g
            let fatFromOil = document.getElementById('oil').value * 14; // 14g fat per tablespoon
            let fatFromMilkTea = document.getElementById('milk_tea').value * 0; // 0g fat per cup
            let fatFromChocolate = document.getElementById('chocolate').value * 5; // 5g fat per cube
            let fatFromCake = document.getElementById('cake').value * 10; // 10g fat per slice
            let fatFromCarbonatedBeverage = document.getElementById('carbonated_beverage').value * 0; // 0g fat per can
            
            // Carb calculations
            let carbsFromRice = document.getElementById('rice').value * 45; // 45g carbs per cup
            let carbsFromBread = document.getElementById('bread').value * 15; // 15g carbs per slice
            let carbsFromSalad = document.getElementById('salad').value * 2; // 2g carbs per cup
            let carbsFromVegetables = document.getElementById('vegetables').value * 5; // 5g carbs per cup
            let carbsFromMilkTea = document.getElementById('milk_tea').value * 10; // 10g carbs per cup
            let carbsFromChocolate = document.getElementById('chocolate').value * 15; // 15g carbs per cube
            let carbsFromCake = document.getElementById('cake').value * 30; // 30g carbs per slice
            let carbsFromCarbonatedBeverage = document.getElementById('carbonated_beverage').value * 40; // 40g carbs per can
            
            // Total macros
            let totalProtein = chicken_breast + chicken_thigh + chicken_wings + chicken_drumsticks + mutton + duck + pigeon + quail + beef + fish + shrimp + dried_fish + egg + protein_shake + rice + bread + oil + salad + vegetables + milk_tea + chocolate + cake + carbonated_beverage;
            let totalFat = fatFromChickenThigh + fatFromChickenDrumsticks + fatFromMutton + fatFromDuck + fatFromPigeon + fatFromQuail + fatFromBeef + fatFromFish + fatFromShrimp + fatFromDriedFish + fatFromOil + fatFromMilkTea + fatFromChocolate + fatFromCake + fatFromCarbonatedBeverage;
            let totalCarbs = carbsFromRice + carbsFromBread + carbsFromSalad + carbsFromVegetables + carbsFromMilkTea + carbsFromChocolate + carbsFromCake + carbsFromCarbonatedBeverage;
            let proteinBalance = requiredProtein - totalProtein;
            
            // Calorie calculation
            let totalCalories = (totalProtein * 4) + (totalCarbs * 4) + (totalFat * 9); // 4 kcal/g for protein and carbs, 9 kcal/g for fat
            
            // Update the display
            document.getElementById('totalProtein').innerText = totalProtein.toFixed(2);
            document.getElementById('totalFat').innerText = totalFat.toFixed(2);
            document.getElementById('totalCarbs').innerText = totalCarbs.toFixed(2);
            document.getElementById('proteinBalance').innerText = proteinBalance.toFixed(2) + " g";
            document.getElementById('proteinBalance').className = proteinBalance > 0 ? "highlight-red" : "";
            document.getElementById('totalCalories').innerText = totalCalories.toFixed(2);
        }

        function calculateTDEE() {
            let weight = parseFloat(document.getElementById('weight').value);
            let height = parseFloat(document.getElementById('height').value);
            let age = parseFloat(document.getElementById('age').value);
            let activityLevel = parseFloat(document.getElementById('activity_level').value);
            let goal = document.getElementById('goal').value;

            // BMR calculation using Mifflin-St Jeor formula
            let bmr = (10 * weight) + (6.25 * height) - (5 * age) + 5; // For men
            let tdee = bmr * activityLevel;
            let targetCalories = goal === "deficit" ? tdee - 500 : tdee; // 500 kcal deficit for weight loss

            document.getElementById('tdee').innerText = tdee.toFixed(2);
            document.getElementById('targetCalories').innerText = targetCalories.toFixed(2);
        }
    </script>
</body>
</html>
