<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Trivilocura</title>
    <style>
        :root {
            --azul-principal: #6a8caf;
            --azul-secundario: #3a5a78;
            --morado-principal: #957fef;
            --morado-secundario: #7161ef;
            --rosa: #ff85a1;
            --verde: #8fbc8f;
            --amarillo: #ffd166;
            --rojo: #ff6b6b;
            --naranja: #ff9e64;
            --blanco: #f8f9fa;
            --negro: #212529;
            --sombra: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Comic Sans MS', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, var(--morado-principal), var(--rosa), var(--naranja));
            color: var(--blanco);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            overflow-x: hidden;
        }

        .container {
            max-width: 900px;
            width: 100%;
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(10px);
            border-radius: 25px;
            padding: 30px;
            box-shadow: var(--sombra);
            margin-top: 20px;
            border: 3px dashed var(--amarillo);
            position: relative;
            overflow: hidden;
        }

        .container::before {
            content: "";
            position: absolute;
            top: -10px;
            left: -10px;
            right: -10px;
            bottom: -10px;
            background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.1), transparent);
            z-index: -1;
            border-radius: 30px;
        }

        h1 {
            text-align: center;
            font-size: 3.5rem;
            margin-bottom: 10px;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            background: linear-gradient(to right, var(--rosa), var(--naranja), var(--amarillo));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            position: relative;
            display: inline-block;
        }

        .corazon {
            color: var(--rojo);
            font-size: 2.5rem;
            position: absolute;
            top: -15px;
            right: -40px;
            animation: latido 1.5s infinite;
            text-shadow: 0 0 10px rgba(255, 107, 107, 0.7);
        }

        @keyframes latido {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }

        .titulo-contenedor {
            display: flex;
            justify-content: center;
            position: relative;
            margin-bottom: 10px;
        }

        .subtitle {
            text-align: center;
            margin-bottom: 30px;
            font-size: 1.3rem;
            color: var(--blanco);
            text-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
        }

        .pantalla {
            display: none;
        }

        .pantalla.activa {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .categorias {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .categoria {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 20px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
            position: relative;
            overflow: hidden;
            transform: rotate(-2deg);
        }

        .categoria:nth-child(even) {
            transform: rotate(2deg);
        }

        .categoria:hover {
            transform: scale(1.05) rotate(0deg);
            border-color: var(--amarillo);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
            z-index: 10;
        }

        .categoria h3 {
            font-size: 1.4rem;
            margin-bottom: 10px;
        }

        .categoria p {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .icono {
            font-size: 2.5rem;
            margin-bottom: 15px;
            filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.2));
        }

        .boton {
            background: linear-gradient(135deg, var(--rosa), var(--naranja));
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 50px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            margin: 10px;
            position: relative;
            overflow: hidden;
        }

        .boton::after {
            content: "";
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(to bottom right, rgba(255,255,255,0.3), rgba(255,255,255,0));
            transform: rotate(30deg);
            transition: all 0.5s;
        }

        .boton:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.3);
        }

        .boton:hover::after {
            transform: rotate(30deg) translate(10%, 10%);
        }

        .boton:active {
            transform: translateY(1px);
        }

        .boton-secundario {
            background: rgba(255, 255, 255, 0.2);
        }

        .centrar {
            display: flex;
            justify-content: center;
            margin-top: 20px;
        }

        .encabezado-juego {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px dashed rgba(255, 255, 255, 0.3);
        }

        .temporizador {
            font-size: 1.5rem;
            font-weight: bold;
            background: rgba(0, 0, 0, 0.3);
            padding: 8px 15px;
            border-radius: 10px;
            border: 2px solid var(--amarillo);
        }

        .puntaje {
            font-size: 1.3rem;
            background: rgba(0, 0, 0, 0.3);
            padding: 8px 15px;
            border-radius: 10px;
            border: 2px solid var(--amarillo);
        }

        .pregunta-contenedor {
            margin-bottom: 30px;
            position: relative;
        }

        .pregunta-texto {
            font-size: 1.4rem;
            margin-bottom: 20px;
            line-height: 1.4;
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 15px;
            border-left: 5px solid var(--rosa);
            text-align: center;
            box-shadow: var(--sombra);
        }

        .categoria-indicador {
            display: inline-block;
            background: var(--morado-principal);
            color: white;
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.9rem;
            margin-bottom: 15px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        .opciones {
            display: grid;
            grid-template-columns: 1fr;
            gap: 15px;
        }

        .opcion {
            background: rgba(255, 255, 255, 0.15);
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 15px;
            padding: 15px 20px;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            position: relative;
            overflow: hidden;
        }

        .opcion::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 5px;
            height: 100%;
            background: var(--morado-principal);
            transform: scaleY(0);
            transition: transform 0.3s;
        }

        .opcion:hover {
            background: rgba(255, 255, 255, 0.25);
            transform: translateX(10px);
        }

        .opcion:hover::before {
            transform: scaleY(1);
        }

        .opcion.correcta {
            background: rgba(143, 188, 143, 0.3);
            border-color: var(--verde);
        }

        .opcion.incorrecta {
            background: rgba(255, 133, 161, 0.3);
            border-color: var(--rosa);
        }

        .letra-opcion {
            display: inline-flex;
            justify-content: center;
            align-items: center;
            width: 30px;
            height: 30px;
            background: var(--morado-principal);
            border-radius: 50%;
            margin-right: 15px;
            font-weight: bold;
            flex-shrink: 0;
        }

        .resultado-final {
            text-align: center;
            padding: 20px;
        }

        .resultado-final h2 {
            font-size: 2.5rem;
            margin-bottom: 20px;
            color: var(--amarillo);
        }

        .puntaje-final {
            font-size: 4rem;
            font-weight: bold;
            margin: 20px 0;
            color: var(--amarillo);
            text-shadow: 0 0 10px rgba(255, 209, 102, 0.5);
        }

        .resumen-correctas {
            font-size: 1.5rem;
            margin: 20px 0;
            color: var(--verde);
        }

        .resumen-errores {
            margin-top: 30px;
            text-align: left;
        }

        .error-item {
            background: rgba(255, 255, 255, 0.1);
            border-left: 4px solid var(--rosa);
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 0 10px 10px 0;
        }

        .error-pregunta {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .error-respuesta {
            color: var(--rosa);
        }

        .correcta-respuesta {
            color: var(--verde);
        }

        .ranking {
            margin-top: 30px;
        }

        .ranking-item {
            display: flex;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 10px;
            border-left: 5px solid var(--naranja);
        }

        .ranking-posicion {
            font-weight: bold;
            color: var(--amarillo);
        }

        .progreso {
            height: 10px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 5px;
            margin: 20px 0;
            overflow: hidden;
        }

        .progreso-barra {
            height: 100%;
            background: linear-gradient(to right, var(--rosa), var(--naranja));
            border-radius: 5px;
            width: 0%;
            transition: width 0.5s ease;
        }

        .oculto {
            display: none;
        }

        .elemento-loco {
            position: absolute;
            font-size: 2rem;
            opacity: 0.1;
            z-index: -1;
            animation: flotar 10s infinite linear;
        }

        @keyframes flotar {
            0% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
            100% { transform: translateY(0) rotate(360deg); }
        }

        .confeti {
            position: fixed;
            width: 10px;
            height: 10px;
            background: #ff85a1;
            opacity: 0.7;
            animation: caer 5s linear infinite;
            z-index: 1000;
        }

        @keyframes caer {
            0% { transform: translateY(-100px) rotate(0deg); }
            100% { transform: translateY(100vh) rotate(360deg); }
        }

        .mensaje-loco {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.8);
            color: var(--amarillo);
            padding: 20px;
            border-radius: 15px;
            font-size: 2rem;
            z-index: 1001;
            display: none;
            text-align: center;
            border: 3px solid var(--rosa);
        }

        .numero-pregunta {
            display: inline-block;
            background: var(--naranja);
            color: white;
            padding: 8px 15px;
            border-radius: 50%;
            font-weight: bold;
            margin-bottom: 15px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
    </style>
</head>
<body>
    <div class="titulo-contenedor">
        <h1>Trivilocura <span class="corazon">💖</span></h1>
    </div>
    <p class="subtitle">¡Pon a prueba tu conocimiento en esta trivia loca!</p>

    <div class="container">
        <!-- Pantalla de inicio -->
        <div id="pantalla-inicio" class="pantalla activa">
            <h2>¡Bienvenido a la Trivilocura!</h2>
            <p>Selecciona una categoría para comenzar. Tienes 15 segundos por pregunta.</p>

            <div class="categorias">
                <div class="categoria" data-categoria="historia">
                    <div class="icono">📜</div>
                    <h3>Historia</h3>
                    <p>10 preguntas sobre eventos históricos</p>
                </div>
                <div class="categoria" data-categoria="ciencia">
                    <div class="icono">🔬</div>
                    <h3>Ciencia</h3>
                    <p>10 preguntas sobre descubrimientos científicos</p>
                </div>
                <div class="categoria" data-categoria="arte">
                    <div class="icono">🎨</div>
                    <h3>Arte y Cultura</h3>
                    <p>10 preguntas sobre artistas y obras</p>
                </div>
                <div class="categoria" data-categoria="geografia">
                    <div class="icono">🌍</div>
                    <h3>Geografía</h3>
                    <p>10 preguntas sobre países y lugares</p>
                </div>
                <div class="categoria" data-categoria="matematicas">
                    <div class="icono">🧮</div>
                    <h3>Matemáticas</h3>
                    <p>10 preguntas sobre números y operaciones</p>
                </div>
                <div class="categoria" data-categoria="ingles">
                    <div class="icono">🔠</div>
                    <h3>Inglés</h3>
                    <p>10 preguntas sobre vocabulario y gramática</p>
                </div>
            </div>
            
            <div class="centrar">
                <button id="btn-ranking" class="boton boton-secundario">Ver Ranking</button>
            </div>
        </div>

        <!-- Pantalla de juego -->
        <div id="pantalla-juego" class="pantalla">
            <div class="encabezado-juego">
                <div class="temporizador" id="temporizador">15</div>
                <div class="puntaje" id="puntaje">Puntos: 0</div>
            </div>
            
            <div class="progreso">
                <div class="progreso-barra" id="progreso-barra"></div>
            </div>
            
            <div class="pregunta-contenedor">
                <div class="numero-pregunta" id="numero-pregunta">1/10</div>
                <div class="categoria-indicador" id="categoria-indicador"></div>
                <div class="pregunta-texto" id="pregunta-texto"></div>
                <div class="opciones" id="opciones-contenedor"></div>
            </div>
            
            <div class="centrar">
                <button id="btn-siguiente" class="boton oculto">Siguiente</button>
            </div>
        </div>

        <!-- Pantalla de resultados -->
        <div id="pantalla-resultados" class="pantalla">
            <div class="resultado-final">
                <h2>¡Juego Terminado!</h2>
                <p>Tu puntuación final es:</p>
                <div class="puntaje-final" id="puntaje-final">0</div>
                <div class="resumen-correctas" id="resumen-correctas"></div>
                
                <div class="resumen-errores" id="resumen-errores">
                    <h3>Preguntas incorrectas:</h3>
                </div>
                
                <div class="centrar">
                    <button id="btn-jugar-otra-vez" class="boton">Jugar Otra Vez</button>
                    <button id="btn-volver-inicio" class="boton boton-secundario">Volver al Inicio</button>
                </div>
            </div>
        </div>

        <!-- Pantalla de ranking -->
        <div id="pantalla-ranking" class="pantalla">
            <h2>Mejores Puntuaciones</h2>
            <div class="ranking" id="ranking-lista">
            </div>
            <div class="centrar">
                <button id="btn-volver-inicio-ranking" class="boton">Volver al Inicio</button>
            </div>
        </div>
    </div>

    <div class="mensaje-loco" id="mensaje-loco"></div>

    <!-- Audio para efectos de sonido -->
    <audio id="click-sound" preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-select-click-1109.mp3" type="audio/mpeg">
    </audio>
    <audio id="correct-sound" preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-correct-answer-tone-2870.mp3" type="audio/mpeg">
    </audio>
    <audio id="wrong-sound" preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-wrong-answer-fail-notification-946.mp3" type="audio/mpeg">
    </audio>

    <script>
        // Base de datos de preguntas por categoría (las 6 categorías con 10 preguntas cada una)
        const preguntas = {
            historia: [
                { pregunta: "¿En qué año cayó el Imperio Romano de Occidente?", opciones: ["476 d.C.", "410 d.C.", "1453 d.C.", "312 d.C."], correcta: 0 },
                { pregunta: "¿Quién descubrió América en 1492?", opciones: ["Cristóbal Colón", "Vasco da Gama", "Fernando de Magallanes", "Américo Vespucio"], correcta: 0 },
                { pregunta: "¿En qué año comenzó la Primera Guerra Mundial?", opciones: ["1914", "1918", "1939", "1945"], correcta: 0 },
                { pregunta: "¿Quién fue el primer presidente de los Estados Unidos?", opciones: ["George Washington", "Thomas Jefferson", "Abraham Lincoln", "John Adams"], correcta: 0 },
                { pregunta: "¿Qué civilización construyó Machu Picchu?", opciones: ["Inca", "Maya", "Azteca", "Olmeca"], correcta: 0 },
                { pregunta: "¿En qué año se firmó la Declaración de Independencia de los Estados Unidos?", opciones: ["1776", "1789", "1812", "1492"], correcta: 0 },
                { pregunta: "¿Quién pintó la Mona Lisa?", opciones: ["Leonardo da Vinci", "Miguel Ángel", "Rafael", "Donatello"], correcta: 0 },
                { pregunta: "¿Qué imperio fue gobernado por Julio César?", opciones: ["Imperio Romano", "Imperio Bizantino", "Imperio Otomano", "Imperio Mongol"], correcta: 0 },
                { pregunta: "¿En qué año terminó la Segunda Guerra Mundial?", opciones: ["1945", "1939", "1918", "1950"], correcta: 0 },
                { pregunta: "¿Quién fue el primer hombre en pisar la Luna?", opciones: ["Neil Armstrong", "Buzz Aldrin", "Yuri Gagarin", "Alan Shepard"], correcta: 0 }
            ],
            ciencia: [
                { pregunta: "¿Cuál es el elemento más abundante en el universo?", opciones: ["Hidrógeno", "Oxígeno", "Carbono", "Helio"], correcta: 0 },
                { pregunta: "¿Quién formuló la teoría de la relatividad?", opciones: ["Albert Einstein", "Isaac Newton", "Stephen Hawking", "Galileo Galilei"], correcta: 0 },
                { pregunta: "¿Cuál es el planeta más grande del sistema solar?", opciones: ["Júpiter", "Saturno", "Neptuno", "Urano"], correcta: 0 },
                { pregunta: "¿Qué científico descubrió la penicilina?", opciones: ["Alexander Fleming", "Louis Pasteur", "Marie Curie", "Robert Koch"], correcta: 0 },
                { pregunta: "¿Cuál es la unidad básica de la vida?", opciones: ["La célula", "El átomo", "La molécula", "El gen"], correcta: 0 },
                { pregunta: "¿Qué partícula subatómica tiene carga positiva?", opciones: ["Protón", "Electrón", "Neutrón", "Fotón"], correcta: 0 },
                { pregunta: "¿Qué gas necesitan las plantas para realizar la fotosíntesis?", opciones: ["Dióxido de carbono", "Oxígeno", "Nitrógeno", "Hidrógeno"], correcta: 0 },
                { pregunta: "¿Cuál es el hueso más largo del cuerpo humano?", opciones: ["Fémur", "Tibia", "Húmero", "Radio"], correcta: 0 },
                { pregunta: "¿Qué planeta es conocido como el planeta rojo?", opciones: ["Marte", "Júpiter", "Venus", "Saturno"], correcta: 0 },
                { pregunta: "¿Qué tipo de animal es una ballena?", opciones: ["Mamífero", "Pez", "Reptil", "Anfibio"], correcta: 0 }
            ],
            arte: [
                { pregunta: "¿Quién pintó 'La noche estrellada'?", opciones: ["Vincent van Gogh", "Pablo Picasso", "Claude Monet", "Salvador Dalí"], correcta: 0 },
                { pregunta: "¿En qué ciudad se encuentra el Museo del Prado?", opciones: ["Madrid", "Barcelona", "París", "Roma"], correcta: 0 },
                { pregunta: "¿Quién es el autor de 'Don Quijote de la Mancha'?", opciones: ["Miguel de Cervantes", "Federico García Lorca", "Gabriel García Márquez", "Pablo Neruda"], correcta: 0 },
                { pregunta: "¿Qué compositor es conocido como 'El rey del vals'?", opciones: ["Johann Strauss II", "Wolfgang Amadeus Mozart", "Ludwig van Beethoven", "Johann Sebastian Bach"], correcta: 0 },
                { pregunta: "¿Qué escultor creó 'El David'?", opciones: ["Miguel Ángel", "Donatello", "Leonardo da Vinci", "Bernini"], correcta: 0 },
                { pregunta: "¿En qué período artístico se encuentra el Renacimiento?", opciones: ["Siglos XV y XVI", "Siglo XIX", "Siglo XVIII", "Siglo XX"], correcta: 0 },
                { pregunta: "¿Quién escribió 'Romeo y Julieta'?", opciones: ["William Shakespeare", "Charles Dickens", "Jane Austen", "Mark Twain"], correcta: 0 },
                { pregunta: "¿Qué pintor es conocido por sus obras de latas de sopa Campbell?", opciones: ["Andy Warhol", "Roy Lichtenstein", "Jackson Pollock", "Keith Haring"], correcta: 0 },
                { pregunta: "¿Qué arquitecto diseñó la Sagrada Familia en Barcelona?", opciones: ["Antoni Gaudí", "Frank Lloyd Wright", "Le Corbusier", "Zaha Hadid"], correcta: 0 },
                { pregunta: "¿Quién compuso 'La Quinta Sinfonía'?", opciones: ["Ludwig van Beethoven", "Wolfgang Amadeus Mozart", "Johann Sebastian Bach", "Franz Schubert"], correcta: 0 }
            ],
            geografia: [
                { pregunta: "¿Cuál es el río más largo del mundo?", opciones: ["Amazonas", "Nilo", "Misisipi", "Yangtsé"], correcta: 0 },
                { pregunta: "¿Cuál es el país más grande del mundo por superficie?", opciones: ["Rusia", "Canadá", "China", "Estados Unidos"], correcta: 0 },
                { pregunta: "¿En qué continente se encuentra Egipto?", opciones: ["África", "Asia", "Europa", "América"], correcta: 0 },
                { pregunta: "¿Cuál es la capital de Australia?", opciones: ["Canberra", "Sídney", "Melbourne", "Brisbane"], correcta: 0 },
                { pregunta: "¿Qué montaña es la más alta del mundo?", opciones: ["Monte Everest", "K2", "Monte Kilimanjaro", "Aconcagua"], correcta: 0 },
                { pregunta: "¿Qué océano es el más grande?", opciones: ["Pacífico", "Atlántico", "Índico", "Ártico"], correcta: 0 },
                { pregunta: "¿Cuál es el desierto más grande del mundo?", opciones: ["Antártida", "Sahara", "Gobi", "Kalahari"], correcta: 0 },
                { pregunta: "¿En qué país se encuentra la Torre Eiffel?", opciones: ["Francia", "Italia", "España", "Alemania"], correcta: 0 },
                { pregunta: "¿Qué país tiene forma de bota?", opciones: ["Italia", "Grecia", "España", "Portugal"], correcta: 0 },
                { pregunta: "¿Cuál es la capital de Japón?", opciones: ["Tokio", "Kioto", "Osaka", "Hiroshima"], correcta: 0 }
            ],
            matematicas: [
                { pregunta: "¿Cuál es el resultado de 15 + 27?", opciones: ["42", "32", "52", "37"], correcta: 0 },
                { pregunta: "¿Cuánto es 8 x 7?", opciones: ["56", "64", "48", "72"], correcta: 0 },
                { pregunta: "¿Qué número es primo?", opciones: ["17", "15", "21", "27"], correcta: 0 },
                { pregunta: "¿Cuál es la raíz cuadrada de 64?", opciones: ["8", "6", "7", "9"], correcta: 0 },
                { pregunta: "¿Qué fracción es equivalente a 0.75?", opciones: ["3/4", "1/2", "2/3", "4/5"], correcta: 0 },
                { pregunta: "¿Cuál es el área de un cuadrado de lado 5 cm?", opciones: ["25 cm²", "20 cm²", "30 cm²", "10 cm²"], correcta: 0 },
                { pregunta: "¿Qué valor tiene π (pi) aproximadamente?", opciones: ["3.1416", "2.7182", "1.6180", "3.2654"], correcta: 0 },
                { pregunta: "¿Cuántos grados tiene un triángulo equilátero?", opciones: ["180°", "90°", "360°", "270°"], correcta: 0 },
                { pregunta: "¿Qué número sigue en la secuencia: 2, 4, 8, 16, ...?", opciones: ["32", "24", "20", "30"], correcta: 0 },
                { pregunta: "¿Cuánto es 100 ÷ 4?", opciones: ["25", "20", "30", "40"], correcta: 0 }
            ],
            ingles: [
                { pregunta: "¿Cómo se dice 'hola' en inglés?", opciones: ["Hello", "Goodbye", "Please", "Thank you"], correcta: 0 },
                { pregunta: "¿Cuál es el plural de 'child'?", opciones: ["Children", "Childs", "Childes", "Childen"], correcta: 0 },
                { pregunta: "¿Qué significa 'apple' en español?", opciones: ["Manzana", "Naranja", "Plátano", "Pera"], correcta: 0 },
                { pregunta: "¿Cuál es el pasado simple de 'go'?", opciones: ["Went", "Goed", "Gone", "Going"], correcta: 0 },
                { pregunta: "¿Cómo se dice 'gracias' en inglés?", opciones: ["Thank you", "Please", "Sorry", "Hello"], correcta: 0 },
                { pregunta: "¿Qué pronombre personal corresponde a 'ellos'?", opciones: ["They", "We", "You", "He"], correcta: 0 },
                { pregunta: "¿Cuál es el comparativo de 'good'?", opciones: ["Better", "Gooder", "More good", "Best"], correcta: 0 },
                { pregunta: "¿Qué palabra significa 'libro' en inglés?", opciones: ["Book", "Look", "Cook", "Hook"], correcta: 0 },
                { pregunta: "¿Cómo se forma el presente continuo en inglés?", opciones: ["Verbo to be + gerundio", "Verbo + ed", "Have + participio", "Will + verbo"], correcta: 0 },
                { pregunta: "¿Qué significa 'house' en español?", opciones: ["Casa", "Caballo", "Hombre", "Hospital"], correcta: 0 }
            ]
        };

        // Variables del juego
        let categoriaActual = '';
        let preguntasActuales = [];
        let preguntaActual = 0;
        let puntuacion = 0;
        let temporizador;
        let tiempoRestante = 15;
        let respuestasIncorrectas = [];
        let respuestasCorrectas = 0;

        // Elementos DOM
        const pantallaInicio = document.getElementById('pantalla-inicio');
        const pantallaJuego = document.getElementById('pantalla-juego');
        const pantallaResultados = document.getElementById('pantalla-resultados');
        const pantallaRanking = document.getElementById('pantalla-ranking');
        
        const btnRanking = document.getElementById('btn-ranking');
        const btnSiguiente = document.getElementById('btn-siguiente');
        const btnJugarOtraVez = document.getElementById('btn-jugar-otra-vez');
        const btnVolverInicio = document.getElementById('btn-volver-inicio');
        const btnVolverInicioRanking = document.getElementById('btn-volver-inicio-ranking');
        
        const preguntaTexto = document.getElementById('pregunta-texto');
        const opcionesContenedor = document.getElementById('opciones-contenedor');
        const temporizadorElemento = document.getElementById('temporizador');
        const puntajeElemento = document.getElementById('puntaje');
        const puntajeFinal = document.getElementById('puntaje-final');
        const resumenCorrectas = document.getElementById('resumen-correctas');
        const resumenErrores = document.getElementById('resumen-errores');
        const rankingLista = document.getElementById('ranking-lista');
        const progresoBarra = document.getElementById('progreso-barra');
        const mensajeLoco = document.getElementById('mensaje-loco');
        const numeroPregunta = document.getElementById('numero-pregunta');
        const categoriaIndicador = document.getElementById('categoria-indicador');

        // Elementos de audio
        const clickSound = document.getElementById('click-sound');
        const correctSound = document.getElementById('correct-sound');
        const wrongSound = document.getElementById('wrong-sound');

        // Función para reproducir sonido
        function playSound(sound) {
            sound.currentTime = 0;
            sound.play().catch(e => {});
        }

        // Crear elementos locos flotantes
        function crearElementosLocos() {
            const elementos = ['🌟', '💫', '🔥', '💖', '🎉', '🚀', '⭐', '✨'];
            const container = document.querySelector('.container');
            
            for (let i = 0; i < 15; i++) {
                const elemento = document.createElement('div');
                elemento.className = 'elemento-loco';
                elemento.textContent = elementos[Math.floor(Math.random() * elementos.length)];
                elemento.style.left = `${Math.random() * 100}%`;
                elemento.style.top = `${Math.random() * 100}%`;
                elemento.style.animationDuration = `${10 + Math.random() * 20}s`;
                container.appendChild(elemento);
            }
        }

        // Crear confeti
        function crearConfeti() {
            const colores = ['#ff85a1', '#ff9e64', '#ffd166', '#957fef', '#8fbc8f'];
            
            for (let i = 0; i < 50; i++) {
                const confeti = document.createElement('div');
                confeti.className = 'confeti';
                confeti.style.left = `${Math.random() * 100}%`;
                confeti.style.animationDuration = `${3 + Math.random() * 4}s`;
                confeti.style.background = colores[Math.floor(Math.random() * colores.length)];
                confeti.style.width = `${5 + Math.random() * 10}px`;
                confeti.style.height = `${5 + Math.random() * 10}px`;
                document.body.appendChild(confeti);
                
                setTimeout(() => {
                    confeti.remove();
                }, 5000);
            }
        }

        // Mostrar mensaje loco
        function mostrarMensajeLoco(mensaje) {
            mensajeLoco.textContent = mensaje;
            mensajeLoco.style.display = 'block';
            
            setTimeout(() => {
                mensajeLoco.style.display = 'none';
            }, 2000);
        }

        // Event listeners para las categorías
        document.querySelectorAll('.categoria').forEach(categoria => {
            categoria.addEventListener('click', () => {
                playSound(clickSound);
                categoriaActual = categoria.getAttribute('data-categoria');
                iniciarJuego();
            });
        });

        // Event listeners para los botones
        btnRanking.addEventListener('click', () => {
            playSound(clickSound);
            mostrarRanking();
        });
        btnSiguiente.addEventListener('click', () => {
            playSound(clickSound);
            siguientePregunta();
        });
        btnJugarOtraVez.addEventListener('click', () => {
            playSound(clickSound);
            iniciarJuego();
        });
        btnVolverInicio.addEventListener('click', () => {
            playSound(clickSound);
            volverInicio();
        });
        btnVolverInicioRanking.addEventListener('click', () => {
            playSound(clickSound);
            volverInicio();
        });

        // Inicializar elementos locos
        crearElementosLocos();

        // Funciones del juego
        function iniciarJuego() {
            // Limpieza preventiva
            clearInterval(temporizador);
            opcionesContenedor.innerHTML = '';
            mensajeLoco.style.display = 'none';
            progresoBarra.style.width = '0%';

            // Reiniciar variables
            preguntasActuales = [...preguntas[categoriaActual]];
            preguntaActual = 0;
            puntuacion = 0;
            respuestasIncorrectas = [];
            respuestasCorrectas = 0;
            
            // Mezclar preguntas
            mezclarPreguntas(preguntasActuales);
            
            // Actualizar UI
            puntajeElemento.textContent = `Puntos: ${puntuacion}`;
            
            // Cambiar pantalla
            cambiarPantalla(pantallaJuego);
            
            // Mostrar primera pregunta
            mostrarPregunta();
        }

        function mezclarPreguntas(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        function mostrarPregunta() {
            // Reiniciar temporizador
            clearInterval(temporizador);
            tiempoRestante = 15;
            temporizadorElemento.textContent = tiempoRestante;
            
            // Actualizar barra de progreso
            progresoBarra.style.width = `${(preguntaActual / preguntasActuales.length) * 100}%`;
            
            // Actualizar número de pregunta
            numeroPregunta.textContent = `${preguntaActual + 1}/${preguntasActuales.length}`;
            
            // Actualizar indicador de categoría
            categoriaIndicador.textContent = categoriaActual.toUpperCase();
            
            // Obtener pregunta actual
            const pregunta = preguntasActuales[preguntaActual];
            
            // Mostrar pregunta
            preguntaTexto.textContent = pregunta.pregunta;
            
            // Limpiar opciones anteriores
            opcionesContenedor.innerHTML = '';
            
            // Crear opciones
            const letras = ['A', 'B', 'C', 'D'];
            pregunta.opciones.forEach((opcion, index) => {
                const elementoOpcion = document.createElement('div');
                elementoOpcion.className = 'opcion';
                elementoOpcion.innerHTML = `
                    <div class="letra-opcion">${letras[index]}</div>
                    <div>${opcion}</div>
                `;
                
                elementoOpcion.addEventListener('click', () => {
                    playSound(clickSound);
                    seleccionarOpcion(index);
                });
                opcionesContenedor.appendChild(elementoOpcion);
            });
            
            // Ocultar botón siguiente
            btnSiguiente.classList.add('oculto');
            
            // Iniciar temporizador
            iniciarTemporizador();
        }

        function iniciarTemporizador() {
            temporizador = setInterval(() => {
                tiempoRestante--;
                temporizadorElemento.textContent = tiempoRestante;
                
                if (tiempoRestante <= 0) {
                    clearInterval(temporizador);
                    tiempoAgotado();
                }
            }, 1000);
        }

        function seleccionarOpcion(indice) {
            clearInterval(temporizador);
            
            const pregunta = preguntasActuales[preguntaActual];
            const opciones = document.querySelectorAll('.opcion');
            
            // Marcar respuesta correcta e incorrecta
            opciones[pregunta.correcta].classList.add('correcta');
            
            if (indice !== pregunta.correcta) {
                opciones[indice].classList.add('incorrecta');
                playSound(wrongSound);
                // Guardar pregunta incorrecta
                respuestasIncorrectas.push({
                    pregunta: pregunta.pregunta,
                    respuestaUsuario: pregunta.opciones[indice],
                    respuestaCorrecta: pregunta.opciones[pregunta.correcta]
                });
            } else {
                playSound(correctSound);
                puntuacion += 10;
                respuestasCorrectas++;
                puntajeElemento.textContent = `Puntos: ${puntuacion}`;
                mostrarMensajeLoco("¡Correcto! 🎉");
            }
            
            // Deshabilitar opciones
            opciones.forEach(opcion => {
                opcion.style.pointerEvents = 'none';
            });
            
            // Mostrar botón siguiente
            btnSiguiente.classList.remove('oculto');
        }

        function tiempoAgotado() {
            const opciones = document.querySelectorAll('.opcion');
            const pregunta = preguntasActuales[preguntaActual];
            
            // Marcar respuesta correcta
            opciones[pregunta.correcta].classList.add('correcta');
            
            // Guardar pregunta como incorrecta por tiempo
            respuestasIncorrectas.push({
                pregunta: pregunta.pregunta,
                respuestaUsuario: "Tiempo agotado",
                respuestaCorrecta: pregunta.opciones[pregunta.correcta]
            });
            
            // Deshabilitar opciones
            opciones.forEach(opcion => {
                opcion.style.pointerEvents = 'none';
            });
            
            // Mostrar botón siguiente
            btnSiguiente.classList.remove('oculto');
            mostrarMensajeLoco("¡Tiempo agotado! ⏰");
        }

        function siguientePregunta() {
            preguntaActual++;
            
            if (preguntaActual < preguntasActuales.length) {
                mostrarPregunta();
            } else {
                terminarJuego();
            }
        }

        function terminarJuego() {
            // Actualizar ranking
            actualizarRanking(puntuacion, categoriaActual);
            
            // Mostrar puntuación final
            puntajeFinal.textContent = puntuacion;
            
            // Mostrar resumen de correctas
            resumenCorrectas.textContent = `${respuestasCorrectas}/${preguntasActuales.length} preguntas correctas`;
            
            // Mostrar preguntas incorrectas
            mostrarErrores();
            
            // Crear confeti si la puntuación es alta
            if (puntuacion >= 70) {
                crearConfeti();
                mostrarMensajeLoco("¡Excelente trabajo! 🏆");
            }
            
            // Cambiar pantalla
            cambiarPantalla(pantallaResultados);
        }

        function mostrarErrores() {
            resumenErrores.innerHTML = '<h3>Preguntas incorrectas:</h3>';
            
            if (respuestasIncorrectas.length === 0) {
                resumenErrores.innerHTML += '<p>¡Perfecto! No tuviste errores.</p>';
                return;
            }
            
            respuestasIncorrectas.forEach(error => {
                const errorElemento = document.createElement('div');
                errorElemento.className = 'error-item';
                errorElemento.innerHTML = `
                    <div class="error-pregunta">${error.pregunta}</div>
                    <div class="error-respuesta">Tu respuesta: ${error.respuestaUsuario}</div>
                    <div class="correcta-respuesta">Respuesta correcta: ${error.respuestaCorrecta}</div>
                `;
                resumenErrores.appendChild(errorElemento);
            });
        }

        function cambiarPantalla(pantalla) {
            // Ocultar todas las pantallas
            document.querySelectorAll('.pantalla').forEach(p => {
                p.classList.remove('activa');
            });
            
            // Mostrar pantalla seleccionada
            pantalla.classList.add('activa');
        }

        function volverInicio() {
            // Detener el temporizador si hay
            clearInterval(temporizador);

            // Reiniciar variables para evitar estados "congelados"
            preguntaActual = 0;
            puntuacion = 0;
            respuestasIncorrectas = [];
            respuestasCorrectas = 0;

            // Limpiar UI relacionada
            opcionesContenedor.innerHTML = '';
            progresoBarra.style.width = '0%';
            puntajeElemento.textContent = `Puntos: 0`;
            mensajeLoco.style.display = 'none';

            // Volver a pantalla inicio
            cambiarPantalla(pantallaInicio);
        }

        function mostrarRanking() {
            // Obtener ranking del localStorage
            const ranking = obtenerRanking();
            
            // Mostrar ranking
            rankingLista.innerHTML = '';
            
            if (ranking.length === 0) {
                rankingLista.innerHTML = '<p>No hay puntuaciones registradas todavía.</p>';
            } else {
                ranking.forEach((puntuacion, index) => {
                    const elementoRanking = document.createElement('div');
                    elementoRanking.className = 'ranking-item';
                    elementoRanking.innerHTML = `
                        <div class="ranking-posicion">${index + 1}.</div>
                        <div>${puntuacion.categoria}</div>
                        <div>${puntuacion.puntos} puntos</div>
                    `;
                    rankingLista.appendChild(elementoRanking);
                });
            }
            
            cambiarPantalla(pantallaRanking);
        }

        function obtenerRanking() {
            const rankingGuardado = localStorage.getItem('triviaRanking');
            return rankingGuardado ? JSON.parse(rankingGuardado) : [];
        }

        function actualizarRanking(puntos, categoria) {
            const ranking = obtenerRanking();
            
            // Agregar nueva puntuación
            ranking.push({
                puntos: puntos,
                categoria: categoria,
                fecha: new Date().toLocaleDateString()
            });
            
            // Ordenar por puntuación (mayor a menor)
            ranking.sort((a, b) => b.puntos - a.puntos);
            
            // Mantener solo las 10 mejores puntuaciones
            if (ranking.length > 10) {
                ranking.splice(10);
            }
            
            // Guardar en localStorage
            localStorage.setItem('triviaRanking', JSON.stringify(ranking));
        }
    </script>
</body>
</html>
