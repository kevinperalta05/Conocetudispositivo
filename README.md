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
    <!-- Tone.js CDN for sound generation -->
    <script src="https://unpkg.com/tone@14.7.58/build/Tone.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1a1a1a; /* Dark background for all pages */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }
        .simulator-container, .activities-section {
            background-color: #2d2d2d; /* Slightly lighter dark background for containers */
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.4); /* Darker shadow */
            padding: 30px;
            max-width: 800px;
            width: 100%;
            text-align: center;
            transition: all 0.3s ease-in-out;
            border: 2px solid #444; /* Darker border */
            color: #e0e0e0; /* Light text for readability */
        }
        .initial-screen {
            background-color: #000; /* Black background for the initial screen */
            color: #fff; /* White text for the initial screen */
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.4);
            padding: 30px;
            max-width: 800px;
            width: 100%;
        }
        .initial-screen h2 {
            color: #fff; /* White color for the title */
        }
        .formula {
            font-size: 3.5rem; /* Larger font size for the formula */
            font-weight: bold;
            color: #ADD8E6; /* Light blue for V and R */
            margin-bottom: 30px;
            display: flex;
            justify-content: center;
            align-items: baseline;
            gap: 15px;
        }
        .formula span {
            font-size: 2.5rem; /* Smaller for I and operators */
            color: #E0E0E0; /* Light gray for I and operators */
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
            color: #E0E0E0; /* Light gray for labels */
            font-size: 1.1rem;
        }
        input[type="range"] {
            width: 80%;
            height: 8px;
            background: #555; /* Darker track */
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
            background: #FF8C00; /* Darker orange/gold for thumb */
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            border: 3px solid #ccc; /* Lighter border on thumb */
        }
        input[type="range"]::-moz-range-thumb {
            width: 25px;
            height: 25px;
            border-radius: 50%;
            background: #FF8C00;
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            border: 3px solid #ccc;
        }
        .result-display {
            background-color: #444; /* Darker background for result */
            padding: 20px;
            border-radius: 10px;
            margin-top: 30px;
            font-size: 2rem;
            font-weight: bold;
            color: #FFD700; /* Gold color for result text */
            box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 90px; /* Fixed height for consistent look */
        }
        .btn {
            background-color: #4C51BF; /* Primary button color (kept for contrast) */
            color: white;
            padding: 12px 25px;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3); /* Darker shadow */
            border: none;
            margin: 0 10px;
        }
        .btn:hover {
            background-color: #363b96;
            transform: translateY(-2px);
        }
        .btn:active {
            transform: translateY(0);
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        }
        .warning-icon {
            color: #EF4444; /* Red for warning (kept vibrant) */
            font-size: 1.2rem;
            margin-left: 10px;
            vertical-align: middle;
        }
        /* Circuit diagram styles */
        .circuit-diagram {
            width: 100%;
            max-width: 300px;
            margin: 0 auto 30px auto;
            border: 2px solid #888; /* Lighter border for circuit */
            background-color: #444; /* Darker background for circuit */
            border-radius: 10px;
            padding: 15px;
        }
        .circuit-diagram svg {
            width: 100%;
            height: auto;
            display: block;
        }
        .wire {
            stroke: #ccc; /* Lighter wires */
            stroke-width: 4;
            fill: none;
            stroke-linecap: round;
            stroke-linejoin: round;
        }
        .battery-cell {
            fill: #666; /* Adjusted grey for battery cells */
            stroke: #ccc;
            stroke-width: 2;
        }
        .battery-terminal {
            stroke: #ccc;
            stroke-width: 2;
            fill: none;
        }
        .battery-label {
            font-size: 12px;
            font-weight: bold;
            fill: #eee; /* Lighter text for labels */
        }
        .resistor {
            fill: #CD853F; /* Peruvian (earthy orange) for resistor body */
            stroke: #ccc;
            stroke-width: 2;
            border-radius: 5px;
        }
        .current-text {
            font-size: 1.2rem;
            font-weight: bold;
            fill: #FFD700; /* Gold for current text */
        }
        .animated-dots {
            fill: #EF4444; /* Red dots for current (kept vibrant) */
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
            fill: #604020; /* Darker orange/brown background for current text */
            stroke: #ff9800; /* Darker orange border */
            stroke-width: 2;
            rx: 5;
            ry: 5;
        }
        .value-display {
            font-size: 0.9rem;
            font-weight: bold;
            fill: #eee; /* Lighter text for values */
            text-anchor: middle;
        }
        .activities-list {
            list-style: none;
            padding: 0;
            margin-top: 20px;
        }
        .activities-list li {
            background-color: #333; /* Darker background for list items */
            border: 1px solid #555; /* Darker border */
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 10px;
            text-align: left;
            font-size: 1.1rem;
            color: #e0e0e0; /* Light text for readability */
            box-shadow: 0 2px 5px rgba(0,0,0,0.15); /* Darker shadow */
        }
        .activities-list li strong {
            color: #FFD700; /* Gold for strong text in list */
        }
        .initial-screen .main-logo-svg {
            width: 90%; /* Make it responsive */
            max-width: 500px; /* Max width for larger screens */
            height: auto;
            margin-bottom: 10px; /* Reduced margin to bring closer to triangle */
            /* Adjusted for perceived centering */
            transform: translateX(-2%); /* Shift left by 2% of its own width */
        }
        .initial-screen .main-logo-svg text {
            font-family: 'Inter', sans-serif; /* Ensure font is applied to SVG text */
            font-weight: bold;
            fill: #FFD700; /* Gold color for main title */
        }
        .initial-screen .main-logo-svg text.subtitle {
            font-size: 14px; /* Smaller for the new, longer subtitle */
            fill: #A9A9A9; /* Darker grey for subtitle */
        }
        /* Style for the gradient line in the SVG */
        .gradient-line {
            stroke-width: 10;
            stroke-linecap: round;
        }
        /* Specific gradient definition */
        .initial-screen .btn {
            background-color: #4C51BF; /* Keep original button style */
            color: white;
            padding: 12px 25px;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
            border: none;
        }
        .initial-screen .btn:hover {
            background-color: #363b96;
            transform: translateY(-2px);
        }

        /* Styles for the Ohm's Law Triangle Animation */
        .ohm-triangle-container {
            width: 200px; /* Size of the triangle animation */
            height: 180px;
            margin-bottom: 20px;
            overflow: visible; /* Allow overflow for animated elements */
        }

        .ohm-triangle-base {
            stroke: #ccc;
            stroke-width: 4;
            fill: none;
        }

        .ohm-triangle-text {
            font-family: 'Inter', sans-serif;
            font-weight: bold;
            font-size: 40px;
            text-anchor: middle;
            fill: #FFD700; /* Gold color for V, I, R letters */
        }

        .ohm-triangle-divider {
            stroke: #888;
            stroke-width: 2;
        }

        .current-dot {
            fill: #EF4444; /* Red color for the moving dot */
            animation: move-dot 4s linear infinite;
        }

        @keyframes move-dot {
            0% {
                cx: 100; cy: 10; /* Top vertex */
            }
            33.33% {
                cx: 190; cy: 170; /* Bottom-right vertex */
            }
            66.66% {
                cx: 10; cy: 170; /* Bottom-left vertex */
            }
            100% {
                cx: 100; cy: 10; /* Back to top vertex */
            }
        }

        /* Multiple dots for continuous flow */
        .current-dot:nth-child(2) {
            animation-delay: 1s;
        }
        .current-dot:nth-child(3) {
            animation-delay: 2s;
        }
        .current-dot:nth-child(4) {
            animation-delay: 3s;
        }
    </style>
</head>
<body>

    <!-- Initial Screen -->
    <div id="initial-screen" class="initial-screen">
        <!-- Recreated Image as SVG for main title -->
        <svg class="main-logo-svg" viewBox="0 0 800 200" fill="none" xmlns="http://www.w3.org/2000/svg">
            <defs>
                <linearGradient id="lineGradient" x1="0%" y1="0%" x2="100%" y2="0%">
                    <stop offset="0%" style="stop-color:#4B0082;stop-opacity:1" /> <!-- Indigo/Purple -->
                    <stop offset="50%" style="stop-color:#FF8C00;stop-opacity:1" /> <!-- DarkOrange -->
                    <stop offset="100%" style="stop-color:#FFD700;stop-opacity:1" /> <!-- Gold -->
                </linearGradient>
            </defs>
            <text x="50%" y="70" text-anchor="middle" font-size="60px" class="main-title">LEY DE OHM</text>
            <text x="50%" y="110" text-anchor="middle" font-size="20px" class="subtitle">SIMULADORES PARA VERIFICAR TUS RESPUESTAS</text>
            <line x1="100" y1="150" x2="700" y2="150" class="gradient-line" stroke="url(#lineGradient)"/>
            <circle cx="700" cy="150" r="15" fill="url(#lineGradient)" opacity="0.8"/>
        </svg>

        <!-- Ohm's Law Triangle Animation -->
        <svg class="ohm-triangle-container" viewBox="0 0 200 180">
            <!-- Triangle outline -->
            <polygon points="100,10 10,170 190,170" class="ohm-triangle-base" />

            <!-- Divider lines -->
            <line x1="10" y1="170" x2="190" y2="170" class="ohm-triangle-divider" />
            <line x1="100" y1="10" x2="100" y2="170" class="ohm-triangle-divider" />

            <!-- Text elements V, I, R -->
            <text x="100" y="70" class="ohm-triangle-text">V</text>
            <text x="55" y="145" class="ohm-triangle-text">I</text>
            <text x="145" y="145" class="ohm-triangle-text">R</text>

            <!-- Animated dots representing current flow -->
            <circle cx="100" cy="10" r="5" class="current-dot"/>
            <circle cx="100" cy="10" r="5" class="current-dot" style="animation-delay: 1s;"/>
            <circle cx="100" cy="10" r="5" class="current-dot" style="animation-delay: 2s;"/>
            <circle cx="100" cy="10" r="5" class="current-dot" style="animation-delay: 3s;"/>
        </svg>

        <button id="start-simulators-btn" class="btn">Iniciar Simuladores</button>
    </div>


    <!-- Main application container (hidden by default) -->
    <div id="app" class="simulator-container hidden">
        <!-- Simulator 1: Calculate Voltaje (V) -->
        <div id="sim-voltage" class="simulator-screen">
            <h2 class="text-3xl font-bold text-white mb-6">Simulador: Calcula el Voltaje (V)</h2>
            <p class="text-lg text-gray-300 mb-8">Ajusta los valores de corriente (I) y resistencia (R) para calcular automáticamente el voltaje (V).</p>

            <div class="formula">
                <span style="color: #ADD8E6;">V</span>
                <span>=</span>
                <span style="font-size: 2.5rem; color: #E0E0E0;">I</span>
                <span style="color: #ADD8E6;">R</span>
            </div>

            <!-- Circuit Diagram -->
            <div class="circuit-diagram">
                <svg viewBox="0 0 300 180">
                    <!-- Wires -->
                    <rect x="20" y="20" width="260" height="140" fill="none" stroke="#ccc" stroke-width="4" rx="10" ry="10"/>
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
                    <rect x="40" y="130" width="220" height="15" rx="5" ry="5" fill="#5F4B30" stroke="#FF8C00" stroke-width="2"/>
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
            <h2 class="text-3xl font-bold text-white mb-6">Simulador: Calcula la Corriente (I)</h2>
            <p class="text-lg text-gray-300 mb-8">Modifica voltaje y resistencia para observar la corriente resultante.</p>

            <div class="formula">
                <span style="font-size: 2.5rem; color: #E0E0E0;">I</span>
                <span>=</span>
                <span style="color: #ADD8E6;">V</span>
                <span>/</span>
                <span style="color: #ADD8E6;">R</span>
            </div>

             <!-- Circuit Diagram (simplified, similar to Sim 1) -->
             <div class="circuit-diagram">
                <svg viewBox="0 0 300 180">
                    <rect x="20" y="20" width="260" height="140" fill="none" stroke="#ccc" stroke-width="4" rx="10" ry="10"/>
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
                    <rect x="40" y="130" width="220" height="15" rx="5" ry="5" fill="#5F4B30" stroke="#FF8C00" stroke-width="2"/>
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
            <h2 class="text-3xl font-bold text-white mb-6">Simulador: Calcula la Resistencia (R)</h2>
            <p class="text-lg text-gray-300 mb-8">Cambia los valores de voltaje y corriente para ver cómo varía la resistencia.</p>

            <div class="formula">
                <span style="color: #ADD8E6;">R</span>
                <span>=</span>
                <span style="color: #ADD8E6;">V</span>
                <span>/</span>
                <span style="font-size: 2.5rem; color: #E0E0E0;">I</span>
            </div>

            <!-- Circuit Diagram (simplified, similar to Sim 1) -->
            <div class="circuit-diagram">
                <svg viewBox="0 0 300 180">
                    <rect x="20" y="20" width="260" height="140" fill="none" stroke="#ccc" stroke-width="4" rx="10" ry="10"/>
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
                    <rect x="40" y="130" width="220" height="15" rx="5" ry="5" fill="#5F4B30" stroke="#FF8C00" stroke-width="2"/>
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
            <h2 class="text-3xl font-bold text-white mb-6">Actividades: Resultados Guardados</h2>
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

        // Elementos principales de la interfaz
        let initialScreen, simVoltage, simCurrent, simResistance, activitiesSection, savedResultsList;
        let startSimulatorsBtn, nextSimBtnV, nextSimBtnI, goToActivitiesBtnR, backToSimsBtn;

        // Tone.js oscillator for electricity sound
        let electricitySynth = null; // Initialize as null
        let timeoutId;

        // Instantiate the synth when the script loads, but don't start it yet.
        try {
            electricitySynth = new Tone.Oscillator({
                frequency: 200, // Starting frequency for the buzz
                type: "sine",
                volume: -20 // Start with a low volume
            }).toDestination();
            console.log("Tone.js Oscillator initialized.");
        } catch (e) {
            console.error("Tone.js Oscillator could not be initialized:", e);
            // If initialization fails, electricitySynth remains null, and sound functions will gracefully skip.
        }

        // Function to play sound
        function playElectricitySound() {
            if (!electricitySynth) {
                console.warn("Electricity synth not initialized. Skipping sound playback.");
                return;
            }

            // Ensure audio context is started by a user gesture first
            if (Tone.context.state !== 'running') {
                Tone.start().then(() => {
                    console.log("Tone.js audio context started.");
                    // After context started, try playing sound again
                    playElectricitySoundInternal();
                }).catch(e => console.error("Error starting Tone.js audio context:", e));
            } else {
                playElectricitySoundInternal();
            }
        }

        // Internal function to actually play the sound after context is ready
        function playElectricitySoundInternal() {
            if (!electricitySynth) return; // Guard for safety if initialization failed
            
            if (electricitySynth.state !== "started") {
                electricitySynth.start();
            }
            // Use Math.random() for frequency variation, mapping to 190-210 Hz
            const randomFreq = 190 + (Math.random() * 20); // Random number between 190 and 210
            electricitySynth.frequency.rampTo(randomFreq, 0.1);
            electricitySynth.volume.rampTo(-12, 0.1); // Increased volume from -15 to -12
            clearTimeout(timeoutId);
            timeoutId = setTimeout(() => {
                electricitySynth.volume.rampTo(-37, 0.5); // Increased faded volume from -40 to -37
                electricitySynth.stop('+0.5'); // Stop after fade out
            }, 100); // Stop after 100ms of no movement
        }

        // Function to handle starting simulators
        function handleStartSimulators() {
            // Tone.start() is now handled within playElectricitySound if not running
            if (initialScreen) initialScreen.classList.add('hidden');
            if (app) app.classList.remove('hidden'); // Show main app container
            if (simVoltage) simVoltage.classList.remove('hidden'); // Show first simulator
        }

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

            playElectricitySound(); // Play sound on update
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

            playElectricitySound(); // Play sound on update
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

            playElectricitySound(); // Play sound on update
        }

        function resetResistanceSimulator() {
            if (!voltageSliderR || !currentSliderR) return;
            voltageSliderR.value = voltageSliderR.min; // Reset to min value
            currentSliderR.value = currentSliderR.min; // Reset to min value
            updateResistance(); // Update display with reset values
            clearAllSavedResults(); // Clear saved results in memory
        }

        // Initialize elements and event listeners after DOM is loaded
        window.onload = function() {
            // Get initial screen elements
            initialScreen = document.getElementById('initial-screen');
            startSimulatorsBtn = document.getElementById('start-simulators-btn');

            // Set up event listener for the start button
            // Note: Audio context must be started by a user gesture, so initAudio is called here
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
            });
        };

    </script>
</body>
</html>
  ELABORADO POR:KEVIN PERALTA
