<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Trivilocura</title>
  <style>
    :root {
      --morado-principal: #9575cd;
      --rosa: #f48fb1;
      --naranja: #ff9e64;
      --blanco: #fff;
      --negro: #212121;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Comic Sans MS', 'Segoe UI', sans-serif;
    }

    body {
      background: linear-gradient(135deg, var(--morado-principal), var(--rosa), var(--naranja));
      color: var(--blanco);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
    }

    .pantalla {
      display: none;
      width: 90%;
      max-width: 600px;
      background: rgba(255,255,255,0.1);
      border-radius: 20px;
      padding: 20px;
      box-shadow: 0 0 20px rgba(0,0,0,0.3);
    }

    .pantalla.activa {
      display: block;
      animation: aparecer 0.6s ease;
    }

    @keyframes aparecer {
      from { opacity: 0; transform: scale(0.95); }
      to { opacity: 1; transform: scale(1); }
    }

    h1 {
      font-size: 2em;
      margin-bottom: 15px;
    }

    .opcion {
      background: rgba(255,255,255,0.2);
      border: 2px solid transparent;
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

    .correcta {
      background: #4caf50 !important;
    }

    .incorrecta {
      background: #e53935 !important;
    }

    .boton {
      background: var(--rosa);
      color: var(--blanco);
      border: none;
      padding: 10px 25px;
      border-radius: 25px;
      margin-top: 15px;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    .boton:hover {
      background: var(--naranja);
    }

    #mensaje-loco {
      font-size: 1.4em;
      margin-top: 10px;
      color: #fff;
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

  </style>
</head>
<body>
  <canvas id="confeti"></canvas>

  <!-- Pantalla de inicio -->
  <div class="pantalla activa" id="pantallaInicio">
    <h1>🎉 Trivilocura 🎉</h1>
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

    let preguntas = [
      { pregunta: "¿En qué año cayó el Imperio Romano de Occidente?", opciones: ["476 d.C.", "1492", "1453", "800 d.C."], correcta: 0 },
      { pregunta: "¿Quién descubrió América?", opciones: ["Cristóbal Colón", "Américo Vespucio", "Leif Erikson", "Marco Polo"], correcta: 0 },
      { pregunta: "¿En qué año comenzó la Primera Guerra Mundial?", opciones: ["1914", "1939", "1929", "1905"], correcta: 0 },
      { pregunta: "¿Quién fue el primer presidente de los Estados Unidos?", opciones: ["George Washington", "Abraham Lincoln", "Thomas Jefferson", "Benjamin Franklin"], correcta: 0 },
      { pregunta: "¿Qué civilización construyó Machu Picchu?", opciones: ["Inca", "Maya", "Azteca", "Olmeca"], correcta: 0 },
      { pregunta: "¿En qué año se firmó la Declaración de Independencia de los Estados Unidos?", opciones: ["1776", "1812", "1789", "1492"], correcta: 0 },
      { pregunta: "¿Quién pintó la Mona Lisa?", opciones: ["Leonardo da Vinci", "Miguel Ángel", "Rafael", "Botticelli"], correcta: 0 },
      { pregunta: "¿Cuál es el planeta más grande del sistema solar?", opciones: ["Júpiter", "Saturno", "Neptuno", "Urano"], correcta: 0 },
      { pregunta: "¿Quién escribió 'Don Quijote de la Mancha'?", opciones: ["Miguel de Cervantes", "Lope de Vega", "Calderón de la Barca", "Góngora"], correcta: 0 },
      { pregunta: "¿Qué país tiene forma de bota?", opciones: ["Italia", "España", "Francia", "Grecia"], correcta: 0 }
    ];

    let preguntaActual = 0;
    let puntuacion = 0;

    function iniciarJuego() {
      puntuacion = 0;
      preguntaActual = 0;
      cambiarPantalla(pantallaJuego);
      mostrarPregunta();
    }

    function mostrarPregunta() {
      btnSiguiente.classList.add('oculto');
      mensajeLoco.textContent = "";

      let pregunta = preguntas[preguntaActual];
      preguntaTexto.textContent = pregunta.pregunta;

      // Mezclar las opciones aleatoriamente
      const indices = [0, 1, 2, 3].sort(() => Math.random() - 0.5);

      opcionesContenedor.innerHTML = "";
      indices.forEach(i => {
        const opcion = document.createElement("div");
        opcion.className = "opcion";
        opcion.textContent = pregunta.opciones[i];
        opcion.onclick = () => seleccionarOpcion(i);
        opcionesContenedor.appendChild(opcion);
      });
    }

    function seleccionarOpcion(i) {
      const opciones = document.querySelectorAll(".opcion");
      opciones.forEach(op => op.style.pointerEvents = "none");

      let pregunta = preguntas[preguntaActual];
      if (i === pregunta.correcta) {
        opciones[i].classList.add("correcta");
        puntuacion += 10;
        mostrarMensaje("¡Correcto! 🎉");
      } else {
        opciones[i].classList.add("incorrecta");
        opciones[pregunta.correcta].classList.add("correcta");
        mostrarMensaje("¡Incorrecto! 😅");
      }

      btnSiguiente.classList.remove('oculto');
    }

    function mostrarMensaje(texto) {
      mensajeLoco.textContent = texto;
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
      puntajeFinal.textContent = `Tu puntuación: ${puntuacion} puntos`;
      if (puntuacion >= 70) {
        crearConfeti();
        mostrarMensaje("¡Excelente trabajo! 🏆");
      }
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
