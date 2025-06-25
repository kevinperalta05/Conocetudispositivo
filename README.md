 <!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>¿DÓNDE VA ESTA PARTE DEL CELULAR?</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            /* Fondo degradado radial más entretenido */
            background: radial-gradient(circle at top left, #e0f7fa, #bbdefb); /* Azul cielo claro a azul un poco más intenso */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background-color: #ffffff;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
            max-width: 900px;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            position: relative;
        }
        h1 {
            color: #1a73e8; /* Azul más vibrante */
            font-size: 2.5rem;
            font-weight: 700;
            text-align: center;
            margin-bottom: 10px;
        }
        h2 {
            color: #5f6368;
            font-size: 1.2rem;
            text-align: center;
            margin-bottom: 20px;
        }
        .instruction {
            color: #3c4043;
            font-size: 1.1rem;
            text-align: center;
            margin-top: 20px;
            font-weight: 500;
        }
        .score-display {
            font-size: 1.4rem;
            font-weight: bold;
            color: #3f51b5; /* Un azul un poco más oscuro para la puntuación */
            margin-bottom: 15px;
        }
        .game-area {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 30px;
            width: 100%;
        }
        @media (min-width: 768px) {
            .game-area {
                flex-direction: row;
                justify-content: space-around;
            }
        }
        .phone-display {
            position: relative;
            width: 250px; /* Ancho fijo para la imagen del celular */
            height: auto;
            min-width: 200px; /* Mínimo para móvil */
            max-width: 90%; /* Máximo para móvil */
            margin: 0 auto;
        }
        .phone-image {
            width: 100%;
            height: auto;
            border-radius: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        .drag-items {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
            padding: 10px;
            border: 2px dashed #a7d9f2; /* Borde suave para la zona de arrastre */
            border-radius: 10px;
            min-height: 150px;
            align-items: center;
        }
        @media (min-width: 768px) {
            .drag-items {
                flex-direction: column;
                min-height: 350px; /* Altura para contener los elementos */
                width: 200px; /* Ancho para las etiquetas */
            }
        }
        .draggable {
            background-color: #e3f2fd; /* Azul muy claro */
            border: 1px solid #90caf9; /* Borde azul claro */
            color: #1a73e8; /* Texto azul */
            padding: 10px 15px;
            border-radius: 8px;
            cursor: grab;
            font-weight: 600;
            user-select: none;
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .draggable:active {
            cursor: grabbing;
            transform: scale(1.05);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
        }
        .dropzone {
            position: absolute;
            background-color: rgba(144, 202, 249, 0.3); /* Azul transparente para la zona de destino */
            border: 2px dashed #1a73e8; /* Borde más oscuro cuando es un objetivo */
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 0.9rem;
            color: #0d47a1;
            font-weight: 600;
            opacity: 0; /* Oculto inicialmente */
            transition: opacity 0.3s ease;
        }
        .dropzone.highlight {
            opacity: 1; /* Visible cuando se arrastra algo */
            background-color: rgba(144, 202, 249, 0.5);
            border-color: #0d47a1;
        }
        .dropzone.dropped {
            opacity: 1; /* Visible cuando ya se ha soltado */
            background-color: #c8e6c9; /* Verde claro al soltar correctamente */
            border-color: #388e3c;
            color: #1b5e20;
        }
        .dropzone.incorrect {
            opacity: 1;
            background-color: #ffcdd2; /* Rojo claro al soltar incorrectamente */
            border-color: #d32f2f;
            color: #b71c1c;
        }

        /* Posiciones y tamaños de las dropzones (ajustar según la nueva imagen) */
        #pantalla-dropzone { top: 9%; left: 16%; width: 68%; height: 80%; } /* Área interior de la pantalla */
        #boton-encendido-dropzone { top: 30%; right: -2%; width: 10%; height: 6%; transform: translateX(100%); } /* Botón lateral derecho superior */
        #volumen-dropzone { top: 40%; right: -2%; width: 10%; height: 10%; transform: translateX(100%); } /* Botones laterales debajo del encendido */
        #camara-frontal-dropzone { top: 4%; left: 45%; width: 10%; height: 3%; } /* Pequeña en la parte superior central */
        #puerto-carga-dropzone { bottom: 0%; left: 40%; width: 20%; height: 5%; } /* Parte inferior central */

        /* Estilo para el botón de reiniciar */
        #restart-button {
            background-color: #4CAF50; /* Un verde amigable */
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.1rem;
            font-weight: bold;
            margin-top: 20px;
            transition: background-color 0.3s ease, transform 0.2s;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        #restart-button:hover {
            background-color: #45a049; /* Verde más oscuro al pasar el ratón */
            transform: translateY(-2px);
        }
        #restart-button:active {
            transform: translateY(0);
        }

        .feedback-message {
            margin-top: 20px;
            padding: 10px 20px;
            border-radius: 10px;
            font-weight: bold;
            font-size: 1.1rem;
            text-align: center;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transform: translateY(10px);
            transition: opacity 0.3s ease, transform 0.3s ease;
        }
        .feedback-message.show {
            opacity: 1;
            transform: translateY(0);
        }
        .feedback-correct {
            background-color: #e6ffe6; /* Verde muy claro */
            color: #2e7d32; /* Verde oscuro */
            border: 1px solid #a5d6a7;
        }
        .feedback-incorrect {
            background-color: #ffe6e6; /* Rojo muy claro */
            color: #c62828; /* Rojo oscuro */
            border: 1px solid #ef9a9a;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>¿DÓNDE VA ESTA PARTE DEL CELULAR?</h1>
        <h2>¡Demuestra cuánto sabes sobre tu celular!</h2>
        <div id="score-display" class="score-display">Puntuación: 0</div>

        <div class="game-area">
            <div class="phone-display">
                <!-- Se mostrará un indicador de carga mientras se genera la imagen -->
                <div id="loading-indicator" class="text-gray-500 text-center py-20">Generando imagen del celular...</div>
                <img id="phone-image" src="" alt="Esquema de un celular" class="phone-image hidden">
                <div id="pantalla-dropzone" class="dropzone" data-target="Pantalla"></div>
                <div id="boton-encendido-dropzone" class="dropzone" data-target="Botón de encendido"></div>
                <div id="volumen-dropzone" class="dropzone" data-target="Volumen + / -"></div>
                <div id="camara-frontal-dropzone" class="dropzone" data-target="Cámara frontal"></div>
                <div id="puerto-carga-dropzone" class="dropzone" data-target="Puerto de carga"></div>
            </div>

            <div class="drag-items" id="drag-items-container">
            </div>
        </div>

        <div id="feedback" class="feedback-message"></div>

        <p class="instruction">Arrastra el nombre de cada parte del celular y colócalo en el lugar correcto.</p>
        <button id="restart-button">Reiniciar Juego</button>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const draggablesData = [
                { id: 'pantalla', text: 'Pantalla' },
                { id: 'boton-encendido', text: 'Botón de encendido' },
                { id: 'volumen', text: 'Volumen + / -' },
                { id: 'camara-frontal', text: 'Cámara frontal' },
                { id: 'puerto-carga', text: 'Puerto de carga' }
            ];

            const dragItemsContainer = document.getElementById('drag-items-container');
            const dropzones = document.querySelectorAll('.dropzone');
            const feedbackElement = document.getElementById('feedback');
            const scoreDisplay = document.getElementById('score-display');
            const restartButton = document.getElementById('restart-button');
            const phoneImageElement = document.getElementById('phone-image');
            const loadingIndicator = document.getElementById('loading-indicator');

            let draggedItem = null;
            let correctDrops = 0;
            let score = 0;
            const totalParts = draggablesData.length;

            // Function to update the score display
            function updateScoreDisplay() {
                scoreDisplay.textContent = `Puntuación: ${score}`;
            }

            // Function to shuffle an array (Fisher-Yates algorithm)
            function shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
            }

            // Function to create and add draggable elements
            function createDraggables() {
                // Clear existing draggables
                dragItemsContainer.innerHTML = '';
                shuffleArray(draggablesData); // Shuffle the order of elements
                draggablesData.forEach(item => {
                    const div = document.createElement('div');
                    div.classList.add('draggable', 'rounded-md', 'shadow-sm');
                    div.setAttribute('draggable', 'true');
                    div.id = `draggable-${item.id}`;
                    div.textContent = item.text;
                    dragItemsContainer.appendChild(div);
                });
                attachDraggableListeners(); // Re-attach listeners to new draggables
            }

            // Function to attach drag event listeners to draggables
            function attachDraggableListeners() {
                document.querySelectorAll('.draggable').forEach(draggable => {
                    draggable.addEventListener('dragstart', (e) => {
                        draggedItem = e.target;
                        e.dataTransfer.setData('text/plain', e.target.textContent);
                        setTimeout(() => {
                            e.target.classList.add('hidden');
                        }, 0);

                        dropzones.forEach(zone => {
                            zone.classList.add('highlight');
                        });
                    });

                    draggable.addEventListener('dragend', (e) => {
                        if (draggedItem) {
                            draggedItem.classList.remove('hidden');
                        }
                        draggedItem = null;

                        dropzones.forEach(zone => {
                            zone.classList.remove('highlight');
                        });
                    });
                });
            }

            // Function to attach drop event listeners to dropzones
            function attachDropzoneListeners() {
                 dropzones.forEach(dropzone => {
                    // Remove old listeners to prevent duplicates on restart
                    dropzone.removeEventListener('dragover', handleDragOver);
                    dropzone.removeEventListener('dragleave', handleDragLeave);
                    dropzone.removeEventListener('drop', handleDrop);

                    // Add new listeners
                    dropzone.addEventListener('dragover', handleDragOver);
                    dropzone.addEventListener('dragleave', handleDragLeave);
                    dropzone.addEventListener('drop', handleDrop);
                });
            }

            // Handlers for drag and drop events
            function handleDragOver(e) {
                e.preventDefault();
                this.classList.add('highlight');
            }

            function handleDragLeave() {
                if (!this.classList.contains('dropped')) {
                    this.classList.remove('highlight');
                }
            }

            function handleDrop(e) {
                e.preventDefault();
                const droppedText = e.dataTransfer.getData('text/plain');
                const targetPart = this.dataset.target;

                this.classList.remove('highlight');

                if (droppedText === targetPart) {
                    this.classList.add('dropped');
                    this.textContent = droppedText;
                    draggedItem.remove();
                    showFeedback('✅ ¡Muy bien! Esa es la parte correcta.', 'correct');
                    score++;
                    correctDrops++;

                    // Mark this dropzone as "completed" to prevent further drops
                    this.dataset.completed = 'true';

                    // Remove specific listeners for this dropzone once an item is correctly dropped
                    this.removeEventListener('dragover', handleDragOver);
                    this.removeEventListener('dragleave', handleDragLeave);
                    this.removeEventListener('drop', handleDrop);


                    if (correctDrops === totalParts) {
                        setTimeout(() => {
                            showFeedback('¡Felicidades! Has identificado todas las partes correctamente.', 'correct');
                        }, 500);
                    }

                } else {
                    this.classList.add('incorrect');
                    this.textContent = '❌';
                    showFeedback('❌ Intenta otra vez.', 'incorrect');
                    score--;
                    if (draggedItem) {
                        draggedItem.classList.remove('hidden');
                    }
                    setTimeout(() => {
                        this.classList.remove('incorrect');
                        this.textContent = '';
                    }, 1000);
                }
                updateScoreDisplay();
            }

            // Function to show feedback
            function showFeedback(message, type) {
                feedbackElement.textContent = message;
                feedbackElement.classList.remove('feedback-correct', 'feedback-incorrect', 'show'); // Ensure old classes are removed
                feedbackElement.classList.add(`feedback-${type}`, 'show');

                setTimeout(() => {
                    feedbackElement.classList.remove('show');
                }, 2000);
            }

            // Function to generate the phone image
            async function generatePhoneImage() {
                loadingIndicator.classList.remove('hidden');
                phoneImageElement.classList.add('hidden');
                const prompt = "A modern flat smartphone with a blank white screen, with side buttons (power and volume) visible. Simple and clean design. Minimalistic, front view, light grey background.";
                const payload = { instances: { prompt: prompt }, parameters: { "sampleCount": 1} };
                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/imagen-3.0-generate-002:predict?key=${apiKey}`;

                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    const result = await response.json();
                    if (result.predictions && result.predictions.length > 0 && result.predictions[0].bytesBase64Encoded) {
                        const imageUrl = `data:image/png;base64,${result.predictions[0].bytesBase64Encoded}`;
                        phoneImageElement.src = imageUrl;
                        phoneImageElement.onload = () => {
                            loadingIndicator.classList.add('hidden');
                            phoneImageElement.classList.remove('hidden');
                        };
                    } else {
                        console.error('Error generating image: No predictions found or malformed response.');
                        // Fallback to a placeholder image if generation fails
                        phoneImageElement.src = "https://placehold.co/250x500/ADD8E6/000000?text=CELULAR";
                        loadingIndicator.classList.add('hidden');
                        phoneImageElement.classList.remove('hidden');
                    }
                } catch (error) {
                    console.error('Error fetching image from API:', error);
                    // Fallback to a placeholder image on network error
                    phoneImageElement.src = "https://placehold.co/250x500/ADD8E6/000000?text=CELULAR";
                    loadingIndicator.classList.add('hidden');
                    phoneImageElement.classList.remove('hidden');
                }
            }

            // Function to reset the game
            function resetGame() {
                score = 0;
                correctDrops = 0;
                updateScoreDisplay();
                
                // Clear dropzones
                dropzones.forEach(zone => {
                    zone.classList.remove('dropped', 'incorrect', 'highlight');
                    zone.textContent = '';
                    delete zone.dataset.completed; // Remove completed status
                });

                createDraggables(); // Recreate draggables
                attachDropzoneListeners(); // Re-attach dropzone listeners
                feedbackElement.classList.remove('show', 'feedback-correct', 'feedback-incorrect'); // Hide feedback
            }

            // Initial game setup
            createDraggables();
            updateScoreDisplay();
            attachDropzoneListeners();
            generatePhoneImage(); // Generate the phone image on load

            // Event listener for restart button
            restartButton.addEventListener('click', resetGame);
        });
    </script>
</body>
</html>
