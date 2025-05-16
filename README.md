<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tienda Capibara</title>
  <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Quicksand', sans-serif;
      background: linear-gradient(to bottom right, #ffe6f0, #e6f7ff, #f0e6ff);
      margin: 0;
      padding: 0;
      color: #444;
    }
    header {
      background-color: #ffd6eb;
      padding: 20px;
      text-align: center;
      font-size: 2em;
      animation: pop 2s infinite;
    }
    @keyframes pop {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.05); }
    }
    .intro {
      text-align: center;
      font-size: 0.9em;
      padding: 10px 30px;
    }
    .recompensa-container, .misiones-container {
      margin: 30px;
    }
    .section-title {
      font-size: 1.5em;
      text-align: center;
      margin-bottom: 10px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 15px;
      justify-items: center;
    }
    .card {
      background-color: #fff0f5;
      border: 2px solid #e3c7f3;
      border-radius: 12px;
      padding: 10px;
      text-align: center;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      position: relative;
    }
    .card::after {
      content: "💖";
      position: absolute;
      top: -10px;
      right: -10px;
      font-size: 1.5em;
    }
    .misiones-scroll {
      overflow-x: auto;
      display: flex;
      gap: 10px;
      padding: 10px;
    }
    .mision {
      flex: 0 0 auto;
      background-color: #e6f0ff;
      border: 2px dashed #c3c3ff;
      border-radius: 10px;
      padding: 10px;
      width: 200px;
      text-align: center;
      position: relative;
    }
    .mision::after {
      content: "🌟";
      position: absolute;
      top: -10px;
      left: -10px;
      font-size: 1.3em;
    }
    button {
      background-color: #dabfff;
      border: none;
      border-radius: 10px;
      padding: 10px 15px;
      font-size: 1em;
      cursor: pointer;
      margin: 10px auto;
      display: block;
      transition: transform 0.2s ease;
    }
    button:hover {
      transform: scale(1.05);
    }
    #capicoins-display {
      text-align: center;
      font-size: 1.2em;
      margin: 10px;
    }
    .stickers {
      text-align: center;
      font-size: 2em;
      margin-top: 10px;
    }
    .info {
      text-align: center;
      font-size: 0.9em;
      color: #666;
      margin: 20px;
    }
    .popup {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      border: 2px solid #dabfff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.3);
      z-index: 999;
      display: none;
      text-align: center;
      font-size: 1.3em;
    }
  </style>
</head>
<body>
  <header>🐄🛍️TIENDA CAPIBARA🛍️🐇</header>
  <div class="intro">
    ¡Hola! Esta tienda virtual es donde podrá canjear sus recompensas y disfrutar de experiencias únicas y personalizadas. Aquí encontrarás una variedad de opciones para que puedas elegir lo que más te guste y te haga sentir especial. ¡Explora y disfruta!
  </div>
  <div class="info">Por cada misión completada ganas 20 Capicoins. Las misiones se reinician cada 24 horas.</div>
  <div id="capicoins-display">Capicoins: <span id="capicoins">0</span></div>
  <button onclick="depositarCapicoins()">🏦 Depositar Capicoins</button>

  <div class="recompensa-container">
    <div class="section-title">🎁 RECOMPENSAS 🎁</div>
    <div class="grid" id="recompensas"></div>
  </div>

  <div class="misiones-container">
    <div class="section-title">🪙 ¿Quieres conseguir más capicoins? 🪙</div>
    <div class="stickers">🍓</div>
    <div class="misiones-scroll" id="misiones"></div>
  </div>

  <div class="stickers">💗💫💖✨💕</div>

  <div class="popup" id="popupFelicidades">¡Felicidades! 🎉</div>

  <audio id="coinSound" src="https://www.myinstants.com/media/sounds/coin.mp3"></audio>
  <audio id="sparkleSound" src="https://www.myinstants.com/media/sounds/magic-chime.mp3"></audio>
  <audio id="bubbleSound" src="https://www.myinstants.com/media/sounds/bubble-pop.mp3"></audio>

  <script>
    let capicoins = parseInt(localStorage.getItem('capicoins')) || 0;

    function actualizarDisplay() {
      document.getElementById('capicoins').textContent = capicoins;
      localStorage.setItem('capicoins', capicoins);
    }

    function mostrarPopup() {
      const popup = document.getElementById('popupFelicidades');
      popup.style.display = 'block';
      setTimeout(() => { popup.style.display = 'none'; }, 2000);
    }

    function canjear(costo, boton) {
      if (capicoins >= costo) {
        capicoins -= costo;
        document.getElementById('sparkleSound').play();
        document.getElementById('bubbleSound').play();
        boton.parentElement.remove();
        mostrarPopup();
        actualizarDisplay();
      } else {
        alert('😿 No tienes los suficientes capicoins 😿');
      }
    }

    function mostrarMisiones() {
      const contenedor = document.getElementById('misiones');
      contenedor.innerHTML = '';
      for (let i = 1; i <= 20; i++) {
        const m = document.createElement('div');
        m.className = 'mision';
        m.innerHTML = `Misión ${i}<br><button onclick="completarMision(this)">Completar</button>`;
        contenedor.appendChild(m);
      }
    }

    function completarMision(boton) {
      capicoins += 20;
      document.getElementById('coinSound').play();
      boton.parentElement.remove();
      actualizarDisplay();
    }

    function depositarCapicoins() {
      let deposito = prompt("¿Cuántos capicoins deseas depositar?");
      let cantidad = parseInt(deposito);
      if (!isNaN(cantidad) && cantidad > 0) {
        capicoins += cantidad;
        actualizarDisplay();
      } else {
        alert("Ingresa una cantidad válida.");
      }
    }

    function generarRecompensas() {
      const contenedor = document.getElementById('recompensas');
      for (let i = 1; i <= 10; i++) {
        const card = document.createElement('div');
        card.className = 'card';
        const costo = (i + 1) * 10 + 50;
        card.innerHTML = `Recompensa ${i}<br>${costo} Capicoins<button onclick="canjear(${costo}, this)">Canjear</button>`;
        contenedor.appendChild(card);
      }
    }

    generarRecompensas();
    mostrarMisiones();
    actualizarDisplay();

    setInterval(mostrarMisiones, 1000 * 60 * 60 * 24); // Reinicia misiones cada 24h
  </script>
</body>
</html>
