<html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simuladores de Ley de Ohm</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Inter Font -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f0f0; /* Light background similar to image */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }
        .simulator-container, .initial-screen {
            background-color: #fff;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
            padding: 30px;
            max-width: 800px;
            width: 100%;
            text-align: center;
            transition: all 0.3s ease-in-out;
            border: 2px solid #ddd;
        }
        .formula {
            font-size: 3.5rem; /* Larger font size for the formula */
            font-weight: bold;
            color: #00008B; /* Dark blue color for V and R, similar to image */
            margin-bottom: 30px;
            display: flex;
            justify-content: center;
            align-items: baseline;
            gap: 15px;
        }
        .formula span {
            font-size: 2.5rem; /* Smaller for I and operators */
            color: #333;
        }
        .slider-group {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-bottom: 25px;
            width: 100%;
        }
        .slider-group label {
            font-weight: bold;
            margin-bottom: 10px;
            color: #4A5568;
            font-size: 1.1rem;
        }
        input[type="range"] {
            width: 80%;
            height: 8px;
            background: #cbd5e0;
            border-radius: 5px;
            outline: none;
            -webkit-appearance: none;
            appearance: none;
            transition: opacity .2s;
            margin-top: 5px;
        }
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 25px;
            height: 25px;
            border-radius: 50%;
            background: #4C51BF; /* Deeper blue for thumb */
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            border: 3px solid #fff; /* White border on thumb */
        }
        input[type="range"]::-moz-range-thumb {
            width: 25px;
            height: 25px;
            border-radius: 50%;
            background: #4C51BF;
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            border: 3px solid #fff;
        }
        .result-display {
            background-color: #e2e8f0; /* Light grey background for result */
            padding: 20px;
            border-radius: 10px;
            margin-top: 30px;
            font-size: 2rem;
            font-weight: bold;
            color: #2D3748;
            box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.06);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 90px; /* Fixed height for consistent look */
        }
        .btn {
            background-color: #4C51BF; /* Primary button color */
            color: white;
            padding: 12px 25px;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
            border: none;
            margin: 0 10px;
        }
        .btn:hover {
            background-color: #363b96;
            transform: translateY(-2px);
        }
        .btn:active {
            transform: translateY(0);
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .warning-icon {
            color: #EF4444; /* Red for warning */
            font-size: 1.2rem;
            margin-left: 10px;
            vertical-align: middle;
        }
        /* Circuit diagram styles - simplified SVG based on the image */
        .circuit-diagram {
            width: 100%;
            max-width: 300px;
            margin: 0 auto 30px auto;
            border: 2px solid #333; /* Border around the circuit area */
            background-color: #fff; /* White background for circuit */
            border-radius: 10px;
            padding: 15px;
        }
        .circuit-diagram svg {
            width: 100%;
            height: auto;
            display: block;
        }
        .wire {
            stroke: #333;
            stroke-width: 4;
            fill: none;
            stroke-linecap: round;
            stroke-linejoin: round;
        }
        .battery-cell {
            fill: #7C7C7C; /* Grey for battery cells */
            stroke: #333;
            stroke-width: 2;
        }
        .battery-terminal {
            stroke: #333;
            stroke-width: 2;
            fill: none;
        }
        .battery-label {
            font-size: 12px;
            font-weight: bold;
            fill: #333;
        }
        .resistor {
            fill: #A0522D; /* Sienna color for resistor body */
            stroke: #333;
            stroke-width: 2;
            border-radius: 5px;
        }
        .current-text {
            font-size: 1.2rem;
            font-weight: bold;
            fill: #000;
        }
        .animated-dots {
            fill: #EF4444; /* Red dots for current */
            animation: current-flow 2s linear infinite;
        }
        @keyframes current-flow {
            0% {
                transform: translateX(0%);
            }
            100% {
                transform: translateX(100%);
            }
        }
        .label-box {
            fill: #ffe0b3; /* Light orange background for current text */
            stroke: #ff9800; /* Darker orange border */
            stroke-width: 2;
            rx: 5;
            ry: 5;
        }
        .value-display {
            font-size: 0.9rem;
            font-weight: bold;
            fill: #333;
            text-anchor: middle;
        }
        .activities-section {
            background-color: #fff;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
            padding: 30px;
            max-width: 800px;
            width: 100%;
            text-align: center;
            border: 2px solid #ddd;
        }
        .activities-list {
            list-style: none;
            padding: 0;
            margin-top: 20px;
        }
        .activities-list li {
            background-color: #f8f8f8;
            border: 1px solid #eee;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 10px;
            text-align: left;
            font-size: 1.1rem;
            color: #333;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }
        .activities-list li strong {
            color: #4C51BF;
        }
        .user-info {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #e2e8f0;
            padding: 10px 20px;
            border-radius: 8px;
            font-size: 0.9rem;
            color: #2D3748;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            z-index: 10;
        }
        .initial-screen {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .initial-screen input[type="text"] {
            padding: 12px 20px;
            margin-bottom: 20px;
            border: 2px solid #cbd5e0;
            border-radius: 8px;
            width: 70%;
            max-width: 300px;
            font-size: 1.1rem;
            text-align: center;
        }
        .initial-screen .btn {
            margin-top: 20px;
            width: auto;
        }
    </style>
</head>
<body>

    <!-- User Information Display (removed session ID display) -->
    <!-- <div id="user-info-display" class="user-info hidden">
        ID de Sesión: <span id="display-user-id"></span>
    </div> -->

    <!-- Initial Screen: Username Input -->
    <div id="initial-screen" class="initial-screen">
        <h2 class="text-3xl font-bold text-gray-800 mb-6">Bienvenido a los Simuladores de Ley de Ohm</h2>
        <button id="start-simulators-btn" class="btn">Iniciar Simuladores</button>
    </div>


    <!-- Main application container (hidden by default) -->
    <div id="app" class="simulator-container hidden">
        <!-- Simulator 1: Calculate Voltaje (V) -->
        <div id="sim-voltage" class="simulator-screen">
            <h2 class="text-3xl font-bold text-gray-800 mb-6">Simulador: Calcula el Voltaje (V)</h2>
            <p class="text-lg text-gray-700 mb-8">Ajusta los valores de corriente (I) y resistencia (R) para calcular automáticamente el voltaje (V).</p>

            <div class="formula">
                <span style="color: #00008B;">V</span>
                <span>=</span>
                <span style="font-size: 2.5rem; color: #333;">I</span>
                <span style="color: #00008B;">R</span>
            </div>

            <!-- Circuit Diagram -->
            <div class="circuit-diagram">
                <svg viewBox="0 0 300 180">
                    <!-- Wires -->
                    <rect x="20" y="20" width="260" height="140" fill="none" stroke="#333" stroke-width="4" rx="10" ry="10"/>
                    <line x1="20" y1="20" x2="20" y2="160" class="wire"/>
                    <line x1="20" y1="160" x2="280" y2="160" class="wire"/>
                    <line x1="280" y1="160" x2="280" y2="20" class="wire"/>
                    <line x1="280" y1="20" x2="20" y2="20" class="wire"/>

                    <!-- Batteries (Cells) -->
                    <rect x="40" y="30" width="30" height="20" rx="3" ry="3" class="battery-cell"/>
                    <text x="55" y="45" class="battery-label">1.5V</text>
                    <rect x="80" y="30" width="30" height="20" rx="3" ry="3" class="battery-cell"/>
                    <text x="95" y="45" class="battery-label">1.5V</text>
                    <rect x="120" y="30" width="30" height="20" rx="3" ry="3" class="battery-cell"/>
                    <text x="135" y="45" class="battery-label">1.5V</text>

                    <!-- Resistor -->
                    <rect x="180" y="30" width="60" height="20" rx="5" ry="5" class="resistor"/>
                    <text x="210" y="45" class="value-display" id="resistor-val-sim1">R Ω</text>


                    <!-- Current display box -->
                    <rect x="70" y="100" width="160" height="30" class="label-box"/>
                    <text x="80" y="120" class="current-text">corriente = <tspan id="current-val-sim1">0.0</tspan> mA</text>

                    <!-- Animated dots for current flow in the resistor-like element-->
                    <rect x="40" y="130" width="220" height="15" rx="5" ry="5" fill="#fefcbf" stroke="#ff9800" stroke-width="2"/>
                    <circle cx="50" cy="137.5" r="3" class="animated-dots"/>
                    <circle cx="80" cy="137.5" r="3" class="animated-dots" style="animation-delay: 0.4s;"/>
                    <circle cx="110" cy="137.5" r="3" class="animated-dots" style="animation-delay: 0.8s;"/>
                    <circle cx="140" cy="137.5" r="3" class="animated-dots" style="animation-delay: 1.2s;"/>
                    <circle cx="170" cy="137.5" r="3" class="animated-dots" style="animation-delay: 1.6s;"/>
                    <circle cx="200" cy="137.5" r="3" class="animated-dots" style="animation-delay: 2.0s;"/>
                    <circle cx="230" cy="137.5" r="3" class="animated-dots" style="animation-delay: 2.4s;"/>

                </svg>
            </div>


            <div class="flex justify-around items-end mt-8">
                <div class="slider-group">
                    <label for="current-slider-v">Corriente (I): <span id="current-value-v">0.1</span> A</label>
                    <input type="range" id="current-slider-v" min="0.1" max="10" value="0.1" step="0.1">
                </div>
                <div class="slider-group">
                    <label for="resistance-slider-v">Resistencia (R): <span id="resistance-value-v">1</span> Ω</label>
                    <input type="range" id="resistance-slider-v" min="1" max="100" value="1" step="1">
                </div>
            </div>

            <div class="result-display mt-8">
                Voltaje (V): <span id="voltage-result">0.1</span> V
            </div>

            <div class="flex justify-center mt-10 space-x-4">
                <button id="save-values-btn-v" class="btn">Guardar este resultado</button>
                <button id="next-sim-btn-v" class="btn">Pasar al siguiente</button>
                <button id="reset-sim-btn-v" class="btn">Reiniciar simulador</button>
            </div>
        </div>

        <!-- Simulator 2: Calculate Current (I) -->
        <div id="sim-current" class="simulator-screen hidden">
            <h2 class="text-3xl font-bold text-gray-800 mb-6">Simulador: Calcula la Corriente (I)</h2>
            <p class="text-lg text-gray-700 mb-8">Modifica voltaje y resistencia para observar la corriente resultante.</p>

            <div class="formula">
                <span style="font-size: 2.5rem; color: #333;">I</span>
                <span>=</span>
                <span style="color: #00008B;">V</span>
                <span>/</span>
                <span style="color: #00008B;">R</span>
            </div>

             <!-- Circuit Diagram (simplified, similar to Sim 1) -->
             <div class="circuit-diagram">
                <svg viewBox="0 0 300 180">
                    <rect x="20" y="20" width="260" height="140" fill="none" stroke="#333" stroke-width="4" rx="10" ry="10"/>
                    <line x1="20" y1="20" x2="20" y2="160" class="wire"/>
                    <line x1="20" y1="160" x2="280" y2="160" class="wire"/>
                    <line x1="280" y1="160" x2="280" y2="20" class="wire"/>
                    <line x1="280" y1="20" x2="20" y2="20" class="wire"/>

                    <!-- Battery -->
                    <rect x="40" y="30" width="60" height="20" rx="3" ry="3" class="battery-cell"/>
                    <text x="70" y="45" class="value-display" id="voltage-val-sim2">V V</text>

                    <!-- Resistor -->
                    <rect x="180" y="30" width="60" height="20" rx="5" ry="5" class="resistor"/>
                    <text x="210" y="45" class="value-display" id="resistor-val-sim2">R Ω</text>

                    <!-- Current display box -->
                    <rect x="70" y="100" width="160" height="30" class="label-box"/>
                    <text x="80" y="120" class="current-text">corriente = <tspan id="current-result">0.0</tspan> A</text>

                    <!-- Animated dots for current flow -->
                    <rect x="40" y="130" width="220" height="15" rx="5" ry="5" fill="#fefcbf" stroke="#ff9800" stroke-width="2"/>
                    <circle cx="50" cy="137.5" r="3" class="animated-dots"/>
                    <circle cx="80" cy="137.5" r="3" class="animated-dots" style="animation-delay: 0.4s;"/>
                    <circle cx="110" cy="137.5" r="3" class="animated-dots" style="animation-delay: 0.8s;"/>
                    <circle cx="140" cy="137.5" r="3" class="animated-dots" style="animation-delay: 1.2s;"/>
                    <circle cx="170" cy="137.5" r="3" class="animated-dots" style="animation-delay: 1.6s;"/>
                    <circle cx="200" cy="137.5" r="3" class="animated-dots" style="animation-delay: 2.0s;"/>
                    <circle cx="230" cy="137.5" r="3" class="animated-dots" style="animation-delay: 2.4s;"/>
                </svg>
            </div>

            <div class="flex justify-around items-end mt-8">
                <div class="slider-group">
                    <label for="voltage-slider-i">Voltaje (V): <span id="voltage-value-i">1</span> V</label>
                    <input type="range" id="voltage-slider-i" min="1" max="100" value="1" step="1">
                </div>
                <div class="slider-group">
                    <label for="resistance-slider-i">Resistencia (R): <span id="resistance-value-i">1</span> Ω</label>
                    <input type="range" id="resistance-slider-i" min="1" max="100" value="1" step="1">
                </div>
            </div>

            <div class="result-display mt-8">
                Corriente (I): <span id="current-result-display">1</span> A
                <span id="warning-icon-i" class="warning-icon hidden" title="Valor de corriente fuera del rango seguro.">⚠️</span>
            </div>

            <div class="flex justify-center mt-10 space-x-4">
                <button id="save-values-btn-i" class="btn">Guardar este resultado</button>
                <button id="next-sim-btn-i" class="btn">Pasar al siguiente</button>
                <button id="reset-sim-btn-i" class="btn">Reiniciar simulador</button>
            </div>
        </div>

        <!-- Simulator 3: Calculate Resistance (R) -->
        <div id="sim-resistance" class="simulator-screen hidden">
            <h2 class="text-3xl font-bold text-gray-800 mb-6">Simulador: Calcula la Resistencia (R)</h2>
            <p class="text-lg text-gray-700 mb-8">Cambia los valores de voltaje y corriente para ver cómo varía la resistencia.</p>

            <div class="formula">
                <span style="color: #00008B;">R</span>
                <span>=</span>
                <span style="color: #00008B;">V</span>
                <span>/</span>
                <span style="font-size: 2.5rem; color: #333;">I</span>
            </div>

            <!-- Circuit Diagram (simplified, similar to Sim 1) -->
            <div class="circuit-diagram">
                <svg viewBox="0 0 300 180">
                    <rect x="20" y="20" width="260" height="140" fill="none" stroke="#333" stroke-width="4" rx="10" ry="10"/>
                    <line x1="20" y1="20" x2="20" y2="160" class="wire"/>
                    <line x1="20" y1="160" x2="280" y2="160" class="wire"/>
                    <line x1="280" y1="160" x2="280" y2="20" class="wire"/>
                    <line x1="280" y1="20" x2="20" y2="20" class="wire"/>

                    <!-- Battery -->
                    <rect x="40" y="30" width="60" height="20" rx="3" ry="3" class="battery-cell"/>
                    <text x="70" y="45" class="value-display" id="voltage-val-sim3">V V</text>

                    <!-- Resistor -->
                    <rect x="180" y="30" width="60" height="20" rx="5" ry="5" class="resistor"/>
                    <text x="210" y="45" class="value-display" id="resistance-result-display">R Ω</text>

                    <!-- Current display box -->
                    <rect x="70" y="100" width="160" height="30" class="label-box"/>
                    <text x="80" y="120" class="current-text">corriente = <tspan id="current-val-sim3">0.0</tspan> A</text>

                    <!-- Animated dots for current flow -->
                    <rect x="40" y="130" width="220" height="15" rx="5" ry="5" fill="#fefcbf" stroke="#ff9800" stroke-width="2"/>
                    <circle cx="50" cy="137.5" r="3" class="animated-dots"/>
                    <circle cx="80" cy="137.5" r="3" class="animated-dots" style="animation-delay: 0.4s;"/>
                    <circle cx="110" cy="137.5" r="3" class="animated-dots" style="animation-delay: 0.8s;"/>
                    <circle cx="140" cy="137.5" r="3" class="animated-dots" style="animation-delay: 1.2s;"/>
                    <circle cx="170" cy="137.5" r="3" class="animated-dots" style="animation-delay: 1.6s;"/>
                    <circle cx="200" cy="137.5" r="3" class="animated-dots" style="animation-delay: 2.0s;"/>
                    <circle cx="230" cy="137.5" r="3" class="animated-dots" style="animation-delay: 2.4s;"/>
                </svg>
            </div>

            <div class="flex justify-around items-end mt-8">
                <div class="slider-group">
                    <label for="voltage-slider-r">Voltaje (V): <span id="voltage-value-r">1</span> V</label>
                    <input type="range" id="voltage-slider-r" min="1" max="100" value="1" step="1">
                </div>
                <div class="slider-group">
                    <label for="current-slider-r">Corriente (I): <span id="current-value-r">0.1</span> A</label>
                    <input type="range" id="current-slider-r" min="0.1" max="10" value="0.1" step="0.1">
                </div>
            </div>

            <div class="result-display mt-8">
                Resistencia (R): <span id="resistance-result">10</span> Ω
            </div>

            <div class="flex justify-center mt-10 space-x-4">
                <button id="save-values-btn-r" class="btn">Guardar este resultado</button>
                <button id="go-to-activities-btn-r" class="btn">Ir a actividades</button>
                <button id="reset-sim-btn-r" class="btn">Reiniciar simulador</button>
            </div>
        </div>

        <!-- Activities Section -->
        <div id="activities-section" class="activities-section hidden">
            <h2 class="text-3xl font-bold text-gray-800 mb-6">Actividades: Resultados Guardados</h2>
            <ul id="saved-results-list" class="activities-list">
                <!-- Saved results will be displayed here -->
                <li class="text-gray-500">No hay resultados guardados todavía.</li>
            </ul>
            <div class="flex justify-center mt-10">
                <button id="back-to-sims-btn" class="btn">Volver a simuladores</button>
            </div>
        </div>
    </div>

    <script>
        // Array para almacenar los resultados en memoria
        let savedResultsInMemory = [];
        // Removed sessionId variable and associated display elements

        // Elementos principales de la interfaz
        let initialScreen, simVoltage, simCurrent, simResistance, activitiesSection, savedResultsList;
        let startSimulatorsBtn, nextSimBtnV, nextSimBtnI, goToActivitiesBtnR, backToSimsBtn;
        // Removed displayUserId, userInfoDisplay

        // Removed displaySessionId function

        // Función para guardar resultados en memoria
        function saveSimulatorResult(simulatorType, inputValues, calculatedResult) {
            savedResultsInMemory.push({
                simulatorType: simulatorType,
                timestamp: new Date().toLocaleString(), // Usar fecha y hora local
                inputValues: inputValues,
                calculatedResult: calculatedResult
            });
            alert(`Resultado de ${simulatorType} guardado exitosamente en memoria!`);
        }

        // Función para limpiar todos los resultados guardados en memoria
        function clearAllSavedResults() {
            savedResultsInMemory = []; // Vacía el arreglo
            if (savedResultsList) {
                savedResultsList.innerHTML = '<li class="text-gray-500">No hay resultados guardados todavía.</li>';
            }
            console.log("Todos los resultados guardados en memoria han sido borrados.");
        }

        // --- Simulator 1: Calculate Voltage (V) ---
        let currentSliderV, resistanceSliderV, currentValueV, resistanceValueV, voltageResult, currentValSim1, resistorValSim1, resetSimBtnV, saveValuesBtnV;

        function updateVoltage() {
            if (!currentSliderV || !resistanceSliderV || !currentValueV || !resistanceValueV || !voltageResult || !currentValSim1 || !resistorValSim1) return;
            const I = parseFloat(currentSliderV.value);
            const R = parseFloat(resistanceSliderV.value);
            const V = I * R;
            currentValueV.textContent = I.toFixed(1);
            resistanceValueV.textContent = R.toFixed(0);
            voltageResult.textContent = V.toFixed(1);

            // Update circuit diagram labels
            currentValSim1.textContent = (I * 1000).toFixed(1); // Convert A to mA for display
            resistorValSim1.textContent = R.toFixed(0) + ' Ω';
        }

        function resetVoltageSimulator() {
            if (!currentSliderV || !resistanceSliderV) return;
            currentSliderV.value = currentSliderV.min; // Reset to min value
            resistanceSliderV.value = resistanceSliderV.min; // Reset to min value
            updateVoltage(); // Update display with reset values
            clearAllSavedResults(); // Clear saved results in memory
        }

        // --- Simulator 2: Calculate Current (I) ---
        let voltageSliderI, resistanceSliderI, voltageValueI, resistanceValueI, currentResultDisplay, warningIconI, voltageValSim2, resistorValSim2, currentResultSim2, resetSimBtnI, saveValuesBtnI;

        function updateCurrent() {
            if (!voltageSliderI || !resistanceSliderI || !voltageValueI || !resistanceValueI || !currentResultDisplay || !warningIconI || !voltageValSim2 || !resistorValSim2 || !currentResultSim2) return;
            const V = parseFloat(voltageSliderI.value);
            const R = parseFloat(resistanceSliderI.value);
            let I = V / R;

            if (isNaN(I) || !isFinite(I)) { // Handle division by zero or invalid numbers
                I = 0; // Or some other default/error value
            }

            voltageValueI.textContent = V.toFixed(0);
            resistanceValueI.textContent = R.toFixed(0);
            currentResultDisplay.textContent = I.toFixed(2); // Display with 2 decimal places

            // Update circuit diagram labels
            voltageValSim2.textContent = V.toFixed(0) + ' V';
            resistorValSim2.textContent = R.toFixed(0) + ' Ω';
            currentResultSim2.textContent = I.toFixed(2);

            // Show warning if current is too high or too low (example ranges)
            if (I > 10 || I < 0.001) { // Example thresholds
                warningIconI.classList.remove('hidden');
            } else {
                warningIconI.classList.add('hidden');
            }
        }

        function resetCurrentSimulator() {
            if (!voltageSliderI || !resistanceSliderI) return;
            voltageSliderI.value = voltageSliderI.min; // Reset to min value
            resistanceSliderI.value = resistanceSliderI.min; // Reset to min value
            updateCurrent(); // Update display with reset values
            clearAllSavedResults(); // Clear saved results in memory
        }

        // --- Simulator 3: Calculate Resistance (R) ---
        let voltageSliderR, currentSliderR, voltageValueR, currentValueR, resistanceResult, voltageValSim3, currentValSim3, resistanceResultDisplaySim3, resetSimBtnR, saveValuesBtnR;

        function updateResistance() {
            if (!voltageSliderR || !currentSliderR || !voltageValueR || !currentValueR || !resistanceResult || !voltageValSim3 || !currentValSim3 || !resistanceResultDisplaySim3) return;
            const V = parseFloat(voltageSliderR.value);
            const I = parseFloat(currentSliderR.value);
            let R = V / I;

            if (isNaN(R) || !isFinite(R)) { // Handle division by zero (I=0) or invalid numbers
                R = 0; // Or some other default/error value
            }

            voltageValueR.textContent = V.toFixed(0);
            currentValueR.textContent = I.toFixed(1);
            resistanceResult.textContent = R.toFixed(2); // Display with 2 decimal places

            // Update circuit diagram labels
            voltageValSim3.textContent = V.toFixed(0) + ' V';
            currentValSim3.textContent = I.toFixed(1);
            resistanceResultDisplaySim3.textContent = R.toFixed(2) + ' Ω';
        }

        function resetResistanceSimulator() {
            if (!voltageSliderR || !currentSliderR) return;
            voltageSliderR.value = voltageSliderR.min; // Reset to min value
            currentSliderR.value = currentSliderR.min; // Reset to min value
            updateResistance(); // Update display with reset values
            clearAllSavedResults(); // Clear saved results in memory
        }

        // Function to handle starting simulators
        function handleStartSimulators() {
            if (initialScreen) initialScreen.classList.add('hidden');
            if (app) app.classList.remove('hidden'); // Show main app container
            if (simVoltage) simVoltage.classList.remove('hidden'); // Show first simulator
            // Removed displaySessionId(sessionId);
        }

        // Initialize elements and event listeners after DOM is loaded
        window.onload = function() {
            // Get initial screen elements
            initialScreen = document.getElementById('initial-screen');
            startSimulatorsBtn = document.getElementById('start-simulators-btn');

            // Get user info display elements (removed references to displayUserId, userInfoDisplay)
            // displayUserId = document.getElementById('display-user-id');
            // userInfoDisplay = document.getElementById('user-info-display');

            // Set up event listener for the start button
            if (startSimulatorsBtn) startSimulatorsBtn.addEventListener('click', handleStartSimulators);

            // All other simulator elements and navigation elements
            // Simulator 1 Elements
            currentSliderV = document.getElementById('current-slider-v');
            resistanceSliderV = document.getElementById('resistance-slider-v');
            currentValueV = document.getElementById('current-value-v');
            resistanceValueV = document.getElementById('resistance-value-v');
            voltageResult = document.getElementById('voltage-result');
            currentValSim1 = document.getElementById('current-val-sim1');
            resistorValSim1 = document.getElementById('resistor-val-sim1');
            resetSimBtnV = document.getElementById('reset-sim-btn-v');
            saveValuesBtnV = document.getElementById('save-values-btn-v');

            // Simulator 1 Event Listeners
            if (currentSliderV) currentSliderV.addEventListener('input', updateVoltage);
            if (resistanceSliderV) resistanceSliderV.addEventListener('input', updateVoltage);
            if (resetSimBtnV) resetSimBtnV.addEventListener('click', resetVoltageSimulator);
            if (saveValuesBtnV) saveValuesBtnV.addEventListener('click', () => {
                const I = parseFloat(currentSliderV.value);
                const R = parseFloat(resistanceSliderV.value);
                const V = I * R;
                saveSimulatorResult('voltage', { I: I, R: R }, { label: 'Voltaje', value: V, unit: 'V' });
            });
            updateVoltage(); // Initial calculation for Voltage simulator

            // Simulator 2 Elements
            voltageSliderI = document.getElementById('voltage-slider-i');
            resistanceSliderI = document.getElementById('resistance-slider-i');
            voltageValueI = document.getElementById('voltage-value-i');
            resistanceValueI = document.getElementById('resistance-value-i');
            currentResultDisplay = document.getElementById('current-result-display');
            warningIconI = document.getElementById('warning-icon-i');
            voltageValSim2 = document.getElementById('voltage-val-sim2');
            resistorValSim2 = document.getElementById('resistor-val-sim2');
            currentResultSim2 = document.getElementById('current-result');
            resetSimBtnI = document.getElementById('reset-sim-btn-i');
            saveValuesBtnI = document.getElementById('save-values-btn-i');

            // Simulator 2 Event Listeners
            if (voltageSliderI) voltageSliderI.addEventListener('input', updateCurrent);
            if (resistanceSliderI) resistanceSliderI.addEventListener('input', updateCurrent);
            if (resetSimBtnI) resetSimBtnI.addEventListener('click', resetCurrentSimulator);
            if (saveValuesBtnI) saveValuesBtnI.addEventListener('click', () => {
                const V = parseFloat(voltageSliderI.value);
                const R = parseFloat(resistanceSliderI.value);
                const I = V / R;
                saveSimulatorResult('current', { V: V, R: R }, { label: 'Corriente', value: I, unit: 'A' });
            });
            updateCurrent(); // Initial calculation for Current simulator

            // Simulator 3 Elements
            voltageSliderR = document.getElementById('voltage-slider-r');
            currentSliderR = document.getElementById('current-slider-r');
            voltageValueR = document.getElementById('voltage-value-r');
            currentValueR = document.getElementById('current-value-r');
            resistanceResult = document.getElementById('resistance-result');
            voltageValSim3 = document.getElementById('voltage-val-sim3');
            currentValSim3 = document.getElementById('current-val-sim3');
            resistanceResultDisplaySim3 = document.getElementById('resistance-result-display');
            resetSimBtnR = document.getElementById('reset-sim-btn-r');
            saveValuesBtnR = document.getElementById('save-values-btn-r');

            // Simulator 3 Event Listeners
            if (currentSliderR) currentSliderR.addEventListener('input', updateResistance);
            if (voltageSliderR) voltageSliderR.addEventListener('input', updateResistance);
            if (resetSimBtnR) resetSimBtnR.addEventListener('click', resetResistanceSimulator);
            if (saveValuesBtnR) saveValuesBtnR.addEventListener('click', () => {
                const V = parseFloat(voltageSliderR.value);
                const I = parseFloat(currentSliderR.value);
                const R = V / I;
                saveSimulatorResult('resistance', { V: V, I: I }, { label: 'Resistencia', value: R, unit: 'Ω' });
            });
            updateResistance(); // Initial calculation for Resistance simulator

            // Navigation Elements
            simVoltage = document.getElementById('sim-voltage');
            simCurrent = document.getElementById('sim-current');
            simResistance = document.getElementById('sim-resistance');
            activitiesSection = document.getElementById('activities-section');
            savedResultsList = document.getElementById('saved-results-list');
            nextSimBtnV = document.getElementById('next-sim-btn-v');
            nextSimBtnI = document.getElementById('next-sim-btn-i');
            goToActivitiesBtnR = document.getElementById('go-to-activities-btn-r');
            backToSimsBtn = document.getElementById('back-to-sims-btn');
            // The main app container:
            app = document.getElementById('app');

            // Navigation Event Listeners
            if (nextSimBtnV) nextSimBtnV.addEventListener('click', () => {
                if (simVoltage) simVoltage.classList.add('hidden');
                if (simCurrent) simCurrent.classList.remove('hidden');
            });

            if (nextSimBtnI) nextSimBtnI.addEventListener('click', () => {
                if (simCurrent) simCurrent.classList.add('hidden');
                if (simResistance) simResistance.classList.remove('hidden');
            });

            if (goToActivitiesBtnR) goToActivitiesBtnR.addEventListener('click', () => {
                if (simResistance) simResistance.classList.add('hidden');
                if (activitiesSection) activitiesSection.classList.remove('hidden');
                
                // Clear previous results display and show loading message
                if (savedResultsList) savedResultsList.innerHTML = '<li class="text-gray-500">Cargando resultados...</li>';

                // Display results from in-memory array
                if (savedResultsList) savedResultsList.innerHTML = ''; // Clear loading message

                if (savedResultsInMemory.length === 0) {
                    if (savedResultsList) savedResultsList.innerHTML = '<li class="text-gray-500">No hay resultados guardados todavía.</li>';
                    return;
                }

                savedResultsInMemory.forEach((data) => {
                    const li = document.createElement('li');
                    
                    li.innerHTML = `
                        <strong>Simulador:</strong> ${data.simulatorType.charAt(0).toUpperCase() + data.simulatorType.slice(1)} <br>
                        <strong>Resultado:</strong> ${data.calculatedResult.label} = ${data.calculatedResult.value.toFixed(2)} ${data.calculatedResult.unit} <br>
                        <span class="text-xs text-gray-400">Guardado el: ${data.timestamp}</span>
                    `;
                    if (savedResultsList) savedResultsList.appendChild(li);
                });
            });

            // Modified backToSimsBtn listener to reset to initial screen and clear in-memory data
            if (backToSimsBtn) backToSimsBtn.addEventListener('click', () => {
                if (activitiesSection) activitiesSection.classList.add('hidden');
                if (app) app.classList.add('hidden'); // Hide the main simulator container
                if (initialScreen) initialScreen.classList.remove('hidden'); // Show the initial screen
                
                clearAllSavedResults(); // Clear results when returning to start
                // Removed sessionId generation and display
            });
        };

    </script>
</body>
</html>
  ELABORADO POR:KEVIN PERALTA
