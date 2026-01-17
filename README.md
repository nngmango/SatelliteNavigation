<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Расчет задержки спутникового сигнала</title>
    <style>
        body {
            font-family: sans-serif;
            margin: 20px;
            background-color: #f4f4f4;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            color: #333;
        }

        .slider-container {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input[type="range"] {
            width: 100%;
        }

        #delay-output {
            font-size: 1.2em;
            margin-top: 10px;
            color: #007BFF;
        }

    </style>
</head>
<body>
    <div class="container">
        <h1>Расчет задержки спутникового сигнала</h1>
        <p>Эта модель позволяет оценить, как различные факторы, такие как частота сигнала и состояние ионосферы, влияют на задержку сигнала спутниковой навигации.</p>

        <div class="slider-container">
            <label for="frequency">Частота сигнала (МГц):</label>
            <input type="range" id="frequency" min="1200" max="1600" value="1575">
            <span id="frequency-value">1575</span> МГц
        </div>

        <div class="slider-container">
            <label for="ionosphere">Уровень ионосферной активности:</label>
            <input type="range" id="ionosphere" min="0" max="10" value="5">
            <span id="ionosphere-value">5</span>
        </div>

         <div class="slider-container">
            <label for="elevation">Угол возвышения спутника (градусы):</label>
            <input type="range" id="elevation" min="0" max="90" value="45">
            <span id="elevation-value">45</span>°
        </div>

        <div>
            <p>Расчетная задержка сигнала:</p>
            <p id="delay-output">0.0 нс</p>
        </div>
    </div>

    <script>
        const frequencySlider = document.getElementById('frequency');
        const frequencyValue = document.getElementById('frequency-value');
        const ionosphereSlider = document.getElementById('ionosphere');
        const ionosphereValue = document.getElementById('ionosphere-value');
        const elevationSlider = document.getElementById('elevation');
        const elevationValue = document.getElementById('elevation-value');
        const delayOutput = document.getElementById('delay-output');


        function calculateDelay(frequency, ionosphereLevel, elevationAngle) {
            // Упрощенные коэффициенты (подберите их, чтобы получить разумные значения задержек)
            const ionoCoefficient = 1000;
            const tropoCoefficient = 10;

            // Ионосферная задержка (обратно пропорциональна квадрату частоты)
            const ionoDelay = ionoCoefficient * ionosphereLevel / (frequency * frequency);

            // Тропосферная задержка (зависит от угла возвышения)
            const tropoDelay = tropoCoefficient / Math.sin(elevationAngle * Math.PI / 180);

            // Общая задержка
            const totalDelay = ionoDelay + tropoDelay;

            return totalDelay.toFixed(2); // Округление до 2 знаков после запятой
        }

        function updateValues() {
            let frequency = parseFloat(frequencySlider.value);
            let ionosphereLevel = parseFloat(ionosphereSlider.value);
            let elevationAngle = parseFloat(elevationSlider.value);

            frequencyValue.textContent = frequency;
            ionosphereValue.textContent = ionosphereLevel;
            elevationValue.textContent = elevationAngle;

            let delay = calculateDelay(frequency, ionosphereLevel, elevationAngle);
            delayOutput.textContent = delay + " нс";
        }

        frequencySlider.addEventListener('input', updateValues);
        ionosphereSlider.addEventListener('input', updateValues);
        elevationSlider.addEventListener('input', updateValues);

        // Initial update
        updateValues();
    </script>
</body>
</html>
