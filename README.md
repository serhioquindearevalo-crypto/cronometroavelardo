<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GANA CON El Avelardo - Cronómetro</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            background-color: #503B76;
            color: white;
            padding: 20px;
            box-sizing: border-box;
        }
        h1 {
            font-size: 4em; /* Más grande */
            font-weight: bold; /* Negrita */
            margin-bottom: 20px;
            text-align: center;
        }
        .logo-container {
            margin-bottom: 30px;
        }
        .logo-container img {
            max-width: 350px; /* Logo más grande */
            height: auto;
            display: block;
            margin: 0 auto;
        }
        #display {
            font-size: 6em; /* Aumentado para el nuevo formato MM:SS.ms */
            margin-bottom: 40px;
            color: white;
            background-color: rgba(0, 0, 0, 0.3);
            padding: 20px 40px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
            font-family: 'Courier New', monospace; 
        }
        .buttons-container {
            display: flex;
            gap: 20px; /* Espacio entre botones */
        }
        .buttons-container button {
            padding: 15px 40px;
            font-size: 1.5em;
            cursor: pointer;
            border: none;
            border-radius: 8px;
            transition: background-color 0.3s, transform 0.2s;
            color: white;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
        }
        .buttons-container button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 8px rgba(0, 0, 0, 0.4);
        }
        .buttons-container button:active {
            transform: translateY(0);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        /* Estilos específicos de los botones */
        #startStop {
            background-color: #4CAF50; /* Verde */
        }
        #startStop.stop-state {
            background-color: #f44336; /* Rojo para PARAR */
        }
        #reset {
            background-color: #2196F3; /* Azul para Reiniciar */
        }
        /* Responsive adjustments */
        @media (max-width: 600px) {
            h1 {
                font-size: 3em;
            }
            .logo-container img {
                max-width: 250px;
            }
            #display {
                font-size: 4em;
                padding: 15px 30px;
            }
            .buttons-container {
                flex-direction: column;
            }
            .buttons-container button {
                font-size: 1.2em;
                padding: 12px 30px;
            }
        }
    </style>
</head>
<body>

    <h1>GANA CON</h1>
    <div class="logo-container">
        <img src="logo_avelardo.png" alt="Logo El Avelardo">
    </div>

    <div id="display">00:00.00</div>
    
    <div class="buttons-container">
        <button id="startStop">INICIAR</button>
        <button id="reset">REINICIAR</button>
    </div>

    <script>
        let startTime;
        let running = false;
        let elapsedTime = 0;
        let intervalId;
        const display = document.getElementById('display');
        const startStopButton = document.getElementById('startStop');
        const resetButton = document.getElementById('reset');

        // Función para formatear el tiempo (MM:SS.ms)
        function formatTime(ms) {
            const totalSeconds = Math.floor(ms / 1000);
            const minutes = Math.floor(totalSeconds / 60) % 60; // Solo minutos y segundos (no horas)
            const seconds = totalSeconds % 60;
            const milliseconds = Math.floor((ms % 1000) / 10); // Dos dígitos de milisegundos

            return (
                String(minutes).padStart(2, '0') + ':' +
                String(seconds).padStart(2, '0') + '.' +
                String(milliseconds).padStart(2, '0')
            );
        }

        function updateTime() {
            elapsedTime = Date.now() - startTime;
            display.textContent = formatTime(elapsedTime);
        }

        // Lógica de INICIAR / PARAR (Pausar/Reanudar)
        const toggleStopwatch = () => {
            if (!running) {
                // Estado: PAUSADO -> INICIAR (Reanudar/Comenzar)
                running = true;
                startTime = Date.now() - elapsedTime;
                intervalId = setInterval(updateTime, 10); 
                startStopButton.textContent = 'PARAR';
                startStopButton.classList.add('stop-state');
            } else {
                // Estado: CORRIENDO -> PARAR (Pausar)
                running = false;
                clearInterval(intervalId);
                startStopButton.textContent = 'INICIAR';
                startStopButton.classList.remove('stop-state');
            }
        };

        // Lógica de Reinicio
        const resetStopwatch = () => {
            clearInterval(intervalId);
            running = false;
            elapsedTime = 0;
            display.textContent = '00:00.00';
            startStopButton.textContent = 'INICIAR';
            startStopButton.classList.remove('stop-state');
        };

        // Event Listeners (Clics y Teclado)
        startStopButton.addEventListener('click', toggleStopwatch);
        resetButton.addEventListener('click', resetStopwatch);

        document.addEventListener('keydown', (event) => {
            // 1. Enter para INICIAR/PARAR
            if (event.key === 'Enter') {
                event.preventDefault(); 
                toggleStopwatch();
            }
            // 2. Barra Espaciadora para REINICIAR
            else if (event.key === ' ' || event.key === 'Spacebar') {
                event.preventDefault(); 
                resetStopwatch();
            }
        });
    </script>
</body>
</html>
