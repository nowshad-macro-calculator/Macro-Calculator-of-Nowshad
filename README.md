
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nowshad's Macro Calculation</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .container { max-width: 500px; margin: auto; }
        input, button { margin: 10px 0; padding: 10px; width: 100%; }
    </style>
</head>
<body>
    <div class="container">
        <h2>Nowshad's Macro Calculation</h2>
        <p>Inspired by ST's nutrition plan</p>
        
        <label for="name">Name:</label>
        <input type="text" id="name">
        
        <label for="age">Age:</label>
        <input type="number" id="age">
        
        <label for="height">Height (cm):</label>
        <input type="number" id="height">
        
        <label for="weight">Weight (kg):</label>
        <input type="number" id="weight">
        
        <h3>BMI: <span id="bmi">0</span></h3>
        
        <h3 style="color: red;">Required Protein: <span id="required_protein_display">0</span> g</h3>
        <label for="required_protein">Enter Required Protein (grams):</label>
        <input type="number" id="required_protein" value="0" oninput="updateRequiredProtein()">
        
        <label for="protein_shake">Protein Shake (scoops):</label>
        <input type="number" id="protein_shake" value="0">
        
        <label for="egg">Eggs (count):</label>
        <input type="number" id="egg" value="0">
        
        <label for="chicken_breast">Chicken Breast (grams):</label>
        <input type="number" id="chicken_breast" value="0">
        
        <label for="chicken_thigh">Chicken Thigh (grams):</label>
        <input type="number" id="chicken_thigh" value="0">
        
        <label for="chicken_wings">Chicken Wings (grams):</label>
        <input type="number" id="chicken_wings" value="0">
        
        <label for="other_chicken">Other Chicken Parts (grams):</label>
        <input type="number" id="other_chicken" value="0">
        
        <label for="salmon">Salmon (grams):</label>
        <input type="number" id="salmon" value="0">
        
        <label for="beef">Beef (grams):</label>
        <input type="number" id="beef" value="0">
        
        <label for="fish">Fish (grams):</label>
        <input type="number" id="fish" value="0">
        
        <label for="shrimp">Shrimp (grams):</label>
        <input type="number" id="shrimp" value="0">
        
        <label for="rice">Rice (cups):</label>
        <input type="number" id="rice" value="0">
        
        <label for="bread">Bread (slices):</label>
        <input type="number" id="bread" value="0">
        
        <label for="oil">Oil (tablespoons):</label>
        <input type="number" id="oil" value="0">
        
        <button onclick="calculateMacros()">Calculate Macros</button>
        
        <h3>Total Protein Intake: <span id="totalProtein">0</span> g</h3>
        <h3>Total Fat Intake: <span id="totalFat">0</span> g</h3>
        <h3>Total Carbohydrate Intake: <span id="totalCarbs">0</span> g</h3>
        <h3>Protein Balance: <span id="proteinBalance">0</span> g</h3>
    </div>

    <script>
        function updateRequiredProtein() {
            document.getElementById('required_protein_display').innerText = document.getElementById('required_protein').value;
        }

        function calculateMacros() {
            let requiredProtein = document.getElementById('required_protein').value;
            let weight = document.getElementById('weight').value;
            let height = document.getElementById('height').value / 100;
            
            let bmi = (weight / (height * height)).toFixed(2);
            document.getElementById('bmi').innerText = bmi;
            
            // Updated protein values per gram or unit
            let chicken_breast = document.getElementById('chicken_breast').value * 0.31; // 31g protein per 100g
            let chicken_thigh = document.getElementById('chicken_thigh').value * 0.26; // 26g protein per 100g
            let chicken_wings = document.getElementById('chicken_wings').value * 0.30; // 30g protein per 100g
            let other_chicken = document.getElementById('other_chicken').value * 0.27; // 27g protein per 100g
            let salmon = document.getElementById('salmon').value * 0.25; // 25g protein per 100g
            let beef = document.getElementById('beef').value * 0.26; // 26g protein per 100g
            let fish = document.getElementById('fish').value * 0.22; // 22g protein per 100g
            let shrimp = document.getElementById('shrimp').value * 0.24; // 24g protein per 100g
            let egg = document.getElementById('egg').value * 6; // 6g protein per egg
            let protein_shake = document.getElementById('protein_shake').value * 25; // 25g protein per scoop
            let rice = document.getElementById('rice').value * 4; // 4g protein per cup
            let bread = document.getElementById('bread').value * 3; // 3g protein per slice
            let oil = document.getElementById('oil').value * 0; // 0g protein per tablespoon
            
            // Fat calculations
            let fatFromChickenThigh = document.getElementById('chicken_thigh').value * 0.08; // 8g fat per 100g
            let fatFromBeef = document.getElementById('beef').value * 0.15; // 15g fat per 100g
            let fatFromFish = document.getElementById('fish').value * 0.05; // 5g fat per 100g
            let fatFromShrimp = document.getElementById('shrimp').value * 0.02; // 2g fat per 100g
            let fatFromOil = document.getElementById('oil').value * 14; // 14g fat per tablespoon
            
            // Carb calculations
            let carbsFromRice = document.getElementById('rice').value * 45; // 45g carbs per cup
            let carbsFromBread = document.getElementById('bread').value * 15; // 15g carbs per slice
            
            let totalProtein = chicken_breast + chicken_thigh + chicken_wings + other_chicken + salmon + beef + fish + shrimp + egg + protein_shake + rice + bread + oil;
            let totalFat = fatFromChickenThigh + fatFromBeef + fatFromFish + fatFromShrimp + fatFromOil;
            let totalCarbs = carbsFromRice + carbsFromBread;
            let proteinBalance = requiredProtein - totalProtein;
            
            document.getElementById('totalProtein').innerText = totalProtein.toFixed(2);
            document.getElementById('totalFat').innerText = totalFat.toFixed(2);
            document.getElementById('totalCarbs').innerText = totalCarbs.toFixed(2);
            document.getElementById('proteinBalance').innerText = proteinBalance.toFixed(2) + " g" + (proteinBalance > 0 ? " more needed" : " exceeded");
        }
    </script>
</body>
</html>
