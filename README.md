<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Trivilocura 💜</title>
  <style>
    :root {
      --morado: #9b5de5;
      --rosa: #f15bb5;
      --naranja: #f9c74f;
      --blanco: #fff;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Comic Sans MS', 'Segoe UI', sans-serif;
    }

    body {
      background: linear-gradient(135deg, var(--morado), var(--rosa), var(--naranja));
      background-size: 400% 400%;
      animation: fondoMov 8s ease infinite;
      color: var(--blanco);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      overflow: hidden;
    }

    @keyframes fondoMov {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    .pantalla {
      display: none;
      width: 90%;
      max-width: 600px;
      background: rgba(255,255,255,0.1);
      border-radius: 20px;
      padding: 25px;
      box-shadow: 0 0 25px rgba(0,0,0,0.4);
    }

    .pantalla.activa {
      display: block;
      animation: aparecer 0.6s ease;
    }

    @keyframes aparecer {
      from { opacity: 0; transform: scale(0.9); }
      to { opacity: 1; transform: scale(1); }
    }

    .opcion {
      background: rgba(255,255,255,0.2);
      border-radius: 12px;
      margin: 10px 0;
      padding: 10px;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .opcion:hover {
      background: rgba(255,255,255,0.4);
      transform: scale(1.03);
    }

    .correcta { background: #4caf50 !important; }
    .incorrecta { background: #e53935 !important; }

    .boton {
      background: var(--rosa);
      color: var(--blanco);
      border: none;
      padding: 10px 25px;
      border-radius: 25px;
      margin-top: 15px;
      cursor: pointer;
      transition: background 0.3s ease, transform 0.2s ease;
    }

    .boton:hover {
      background: var(--naranja);
      transform: scale(1.05);
    }

    #mensaje-loco {
      font-size: 1.3em;
      margin-top: 10px;
      text-shadow: 0 0 5px #000;
    }

    #confeti {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 999;
    }

    select {
      background: rgba(255,255,255,0.2);
      border: none;
      border-radius: 15px;
      padding: 10px;
      color: var(--blanco);
      font-size: 1em;
      margin-top: 15px;
      text-align: center;
    }
    option {
      color: black;
    }
  </style>
</head>
<body>
  <canvas id="confeti"></canvas>

  <!-- Pantalla de inicio -->
  <div class="pantalla activa" id="pantallaInicio">
    <h1>🎉 Trivilocura 🎉</h1>
    <p>Elige una categoría para comenzar:</p>
    <select id="categoriaSelect">
      <option value="historia">📜 Historia</option>
      <option value="ciencia">🔬 Ciencia</option>
      <option value="cultura">🎭 Cultura General</option>
      <option value="deportes">⚽ Deportes</option>
      <option value="mixta">🎲 Mixta</option>
    </select>
    <br>
    <button class="boton" onclick="iniciarJuego()">Jugar</button>
  </div>

  <!-- Pantalla del juego -->
  <div class="pantalla" id="pantallaJuego">
    <h1 id="preguntaTexto">Pregunta</h1>
    <div id="opcionesContenedor"></div>
    <div id="mensaje-loco"></div>
    <button id="btnSiguiente" class="boton oculto" onclick="siguientePregunta()">Siguiente</button>
  </div>

  <!-- Pantalla de resultados -->
  <div class="pantalla" id="pantallaResultados">
    <h1>Resultados 🎯</h1>
    <p id="puntajeFinal"></p>
    <button class="boton" onclick="volverInicio()">Volver al inicio</button>
  </div>

  <script>
    const pantallaInicio = document.getElementById('pantallaInicio');
    const pantallaJuego = document.getElementById('pantallaJuego');
    const pantallaResultados = document.getElementById('pantallaResultados');
    const preguntaTexto = document.getElementById('preguntaTexto');
    const opcionesContenedor = document.getElementById('opcionesContenedor');
    const mensajeLoco = document.getElementById('mensaje-loco');
    const btnSiguiente = document.getElementById('btnSiguiente');
    const puntajeFinal = document.getElementById('puntajeFinal');
    const categoriaSelect = document.getElementById('categoriaSelect');

    // 🎯 Base de preguntas por categoría
    const categorias = {
      historia: [
        { pregunta: "¿En qué año cayó el Imperio Romano de Occidente?", opciones: ["476 d.C.", "1492", "1453", "800 d.C."], correcta: 0 },
        { pregunta: "¿Quién descubrió América?", opciones: ["Cristóbal Colón", "Américo Vespucio", "Leif Erikson", "Marco Polo"], correcta: 0 },
        { pregunta: "¿En qué año comenzó la Primera Guerra Mundial?", opciones: ["1914", "1939", "1929", "1905"], correcta: 0 },
        { pregunta: "¿Cuándo terminó la Segunda Guerra Mundial?", opciones: ["1945", "1939", "1950", "1960"], correcta: 0 }
      ],
      ciencia: [
        { pregunta: "¿Cuál es el planeta más grande del sistema solar?", opciones: ["Júpiter", "Saturno", "Neptuno", "Urano"], correcta: 0 },
        { pregunta: "¿Qué gas respiramos para vivir?", opciones: ["Oxígeno", "Hidrógeno", "Nitrógeno", "Dióxido de carbono"], correcta: 0 },
        { pregunta: "¿Qué órgano bombea la sangre?", opciones: ["Corazón", "Pulmones", "Hígado", "Riñón"], correcta: 0 },
        { pregunta: "¿Cuál es el hueso más largo del cuerpo humano?", opciones: ["Fémur", "Húmero", "Tibia", "Peroné"], correcta: 0 }
      ],
      cultura: [
        { pregunta: "¿Quién escribió 'Don Quijote de la Mancha'?", opciones: ["Miguel de Cervantes", "Lope de Vega", "Calderón de la Barca", "Góngora"], correcta: 0 },
        { pregunta: "¿Qué país tiene forma de bota?", opciones: ["Italia", "España", "Francia", "Grecia"], correcta: 0 },
        { pregunta: "¿Quién pintó la Mona Lisa?", opciones: ["Leonardo da Vinci", "Miguel Ángel", "Rafael", "Botticelli"], correcta: 0 },
        { pregunta: "¿Qué civilización construyó Machu Picchu?", opciones: ["Inca", "Maya", "Azteca", "Olmeca"], correcta: 0 }
      ],
      deportes: [
        { pregunta: "¿Cuántos jugadores tiene un equipo de fútbol?", opciones: ["11", "9", "7", "10"], correcta: 0 },
        { pregunta: "¿En qué deporte se usa una raqueta?", opciones: ["Tenis", "Golf", "Baloncesto", "Natación"], correcta: 0 },
        { pregunta: "¿Qué país ganó el Mundial 2014?", opciones: ["Alemania", "Brasil", "Argentina", "España"], correcta: 0 },
        { pregunta: "¿Dónde se celebraron los JJ.OO. de 2016?", opciones: ["Río de Janeiro", "Tokio", "Londres", "Pekín"], correcta: 0 }
      ]
    };

    let preguntas = [];
    let preguntaActual = 0;
    let puntuacion = 0;
    let opcionesActuales = [];

    function iniciarJuego() {
      puntuacion = 0;
      preguntaActual = 0;
      const seleccion = categoriaSelect.value;

      if (seleccion === "mixta") {
        preguntas = Object.values(categorias).flat().sort(() => Math.random() - 0.5).slice(0, 10);
      } else {
        preguntas = categorias[seleccion];
      }

      cambiarPantalla(pantallaJuego);
      mostrarPregunta();
    }

    function mostrarPregunta() {
      btnSiguiente.classList.add('oculto');
      mensajeLoco.textContent = "";

      let pregunta = preguntas[preguntaActual];
      preguntaTexto.textContent = pregunta.pregunta;

      // Mezclar opciones
      const indices = [0, 1, 2, 3].sort(() => Math.random() - 0.5);
      opcionesActuales = indices.map(i => ({
        texto: pregunta.opciones[i],
        esCorrecta: i === pregunta.correcta
      }));

      opcionesContenedor.innerHTML = "";
      opcionesActuales.forEach((opcion, i) => {
        const div = document.createElement("div");
        div.className = "opcion";
        div.textContent = opcion.texto;
        div.onclick = () => seleccionarOpcion(i);
        opcionesContenedor.appendChild(div);
      });
    }

    function seleccionarOpcion(i) {
      const opciones = document.querySelectorAll(".opcion");
      opciones.forEach(op => op.style.pointerEvents = "none");

      if (opcionesActuales[i].esCorrecta) {
        opciones[i].classList.add("correcta");
        puntuacion += 10;
        mensajeLoco.textContent = "¡Correcto! 🎉";
      } else {
        opciones[i].classList.add("incorrecta");
        const correctaIndex = opcionesActuales.findIndex(o => o.esCorrecta);
        opciones[correctaIndex].classList.add("correcta");
        mensajeLoco.textContent = "¡Incorrecto! 😅";
      }

      btnSiguiente.classList.remove('oculto');
    }

    function siguientePregunta() {
      preguntaActual++;
      if (preguntaActual < preguntas.length) {
        mostrarPregunta();
      } else {
        terminarJuego();
      }
    }

    function terminarJuego() {
      puntajeFinal.textContent = `Tu puntuación final: ${puntuacion} puntos`;
      if (puntuacion >= 70) crearConfeti();
      cambiarPantalla(pantallaResultados);
    }

    function volverInicio() {
      cambiarPantalla(pantallaInicio);
    }

    function cambiarPantalla(pantalla) {
      document.querySelectorAll('.pantalla').forEach(p => p.classList.remove('activa'));
      pantalla.classList.add('activa');
    }

    // 🎉 Confeti
    const canvas = document.getElementById("confeti");
    const ctx = canvas.getContext("2d");
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    let confetis = [];

    function crearConfeti() {
      confetis = Array.from({ length: 150 }).map(() => ({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height - canvas.height,
        r: Math.random() * 6 + 4,
        color: `hsl(${Math.random() * 360}, 100%, 70%)`,
        velocidad: Math.random() * 4 + 2
      }));
      animarConfeti();
    }

    function animarConfeti() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      confetis.forEach(c => {
        c.y += c.velocidad;
        if (c.y > canvas.height) c.y = -10;
        ctx.beginPath();
        ctx.arc(c.x, c.y, c.r, 0, 2 * Math.PI);
        ctx.fillStyle = c.color;
        ctx.fill();
      });
      requestAnimationFrame(animarConfeti);
    }
  </script>
</body>
</html>
