# Truck-Stick
Instantly calculate an athlete's truck stick (momentum) using their Fly 10 time, MPH or M/S and body weight!
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Truck Stick Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
        }
        .input-group {
            margin: 10px 0;
        }
        label {
            display: inline-block;
            width: 180px;
        }
        button {
            margin-top: 10px;
            padding: 5px 15px;
        }
        #result {
            margin-top: 15px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h2>Truck Stick Calculator</h2>
    
    <div class="input-group">
        <label for="timeYards">10 Yard Fly Time (seconds):</label>
        <input type="number" id="timeYards" step="0.01" min="0">
    </div>
    
    <div class="input-group">
        <label for="timeMeters">10 Meter Fly Time (seconds):</label>
        <input type="number" id="timeMeters" step="0.01" min="0">
    </div>
    
    <div class="input-group">
        <label for="mph">Speed (mph):</label>
        <input type="number" id="mph" step="0.1" min="0">
    </div>
    
    <div class="input-group">
        <label for="ms">Speed (m/s):</label>
        <input type="number" id="ms" step="0.1" min="0">
    </div>
    
    <div class="input-group">
        <label for="weight">Weight (lbs):</label>
        <input type="number" id="weight" step="0.1" min="0" required>
    </div>
    
    <button onclick="calculateTruckStick()">Calculate Truck Stick</button>
    
    <div id="result"></div>

    <script>
        function calculateTruckStick() {
            // Get input values
            const timeYards = parseFloat(document.getElementById('timeYards').value);
            const timeMeters = parseFloat(document.getElementById('timeMeters').value);
            const mph = parseFloat(document.getElementById('mph').value);
            const ms = parseFloat(document.getElementById('ms').value);
            const weightLbs = parseFloat(document.getElementById('weight').value);
            
            // Validate weight
            if (isNaN(weightLbs) || weightLbs <= 0) {
                document.getElementById('result').innerHTML = 
                    'Please enter a valid positive weight.';
                return;
            }
            
            // Count valid speed inputs
            const validInputs = [
                !isNaN(timeYards) && timeYards > 0,
                !isNaN(timeMeters) && timeMeters > 0,
                !isNaN(mph) && mph > 0,
                !isNaN(ms) && ms > 0
            ].filter(Boolean).length;
            
            if (validInputs === 0) {
                document.getElementById('result').innerHTML = 
                    'Please enter at least one valid positive speed value.';
                return;
            }
            if (validInputs > 1) {
                document.getElementById('result').innerHTML = 
                    'Please enter only one speed value (10 Yard Fly, 10 Meter Fly, MPH, or M/S).';
                return;
            }

            // Calculate speed in m/s based on provided input
            let speedMs, speedType;
            if (!isNaN(timeYards) && timeYards > 0) {
                const distanceYardsMeters = 10 * 0.9144;  // 10 yards to meters
                speedMs = distanceYardsMeters / timeYards;
                speedType = "10 Yard Fly";
            } else if (!isNaN(timeMeters) && timeMeters > 0) {
                speedMs = 10 / timeMeters;  // 10 meters directly
                speedType = "10 Meter Fly";
            } else if (!isNaN(mph) && mph > 0) {
                speedMs = mph * 0.44704;  // mph to m/s
                speedType = "MPH";
            } else if (!isNaN(ms) && ms > 0) {
                speedMs = ms;  // already in m/s
                speedType = "M/S";
            }
            
            // Convert weight to kg
            const massKg = weightLbs * 0.453592;
            
            // Calculate momentum (p = m × v) in kg·m/s
            const momentum = massKg * speedMs;
            
            // Display result
            document.getElementById('result').innerHTML = 
                `Truck Stick Value: ${momentum.toFixed(2)} kg·m/s<br>` +
                `Speed: ${speedMs.toFixed(2)} m/s (based on ${speedType})`;
        }
    </script>
</body>
</html>
