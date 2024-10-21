<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Juego QR</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background-image: url('/mnt/data/image.png'); /* Usamos la ruta de la imagen subida */
      background-size: cover;
      background-position: center;
      color: white;
    }
    .container {
      text-align: center;
      background-color: rgba(0, 0, 0, 0.7); /* Fondo oscuro semitransparente */
      padding: 20px;
      border-radius: 10px;
    }
    .hidden {
      display: none;
    }
    .player-select, .customization, .score-counter {
      margin-top: 20px;
    }
    .button {
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
      margin: 5px;
    }
    .button:hover {
      background-color: #0056b3;
    }
    .score {
      font-size: 48px;
    }
    .score-buttons {
      display: flex;
      justify-content: center;
      gap: 10px;
    }
    #player-photo-preview {
      width: 100px;
      height: 100px;
      border-radius: 50%;
      object-fit: cover;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Bienvenido al Juego</h1>

    <!-- Botón de empezar -->
    <div id="start-screen">
      <button id="start-btn" class="button">Empezar</button>
    </div>

    <!-- Selección de jugador -->
    <div id="player-select" class="player-select hidden">
      <h2>Selecciona tu jugador</h2>
      <button class="button" onclick="selectPlayer(1)">Jugador 1</button>
      <button class="button" onclick="selectPlayer(2)">Jugador 2</button>
    </div>

    <!-- Personalización -->
    <div id="customization" class="customization hidden">
      <h2>Personaliza tu jugador</h2>
      <label for="name">Nombre:</label>
      <input type="text" id="player-name" placeholder="Escribe tu nombre"><br><br>
      
      <label for="color">Escoge un color:</label>
      <input type="color" id="player-color" value="#ff0000"><br><br>

      <label for="photo">Sube una foto:</label>
      <input type="file" id="player-photo" accept="image/*"><br><br>
      
      <button id="start-game-btn" class="button">Iniciar Juego</button>
    </div>

    <!-- Contador de puntos -->
    <div id="score-counter" class="score-counter hidden">
      <h2>Puntos del Jugador <span id="player-name-display"></span></h2>
      <img id="player-photo-preview" src="" alt="Foto del Jugador" class="hidden"><br>
      <div class="score" id="score">0</div>

      <div class="score-buttons">
        <button id="add-point-btn" class="button">+1</button>
        <button id="subtract-point-btn" class="button">-1</button>
      </div>

      <!-- Puntos del otro jugador -->
      <h3>Puntos del Jugador Oponente</h3>
      <div class="score" id="opponent-score">0</div>
    </div>
  </div>

  <script>
    let score = 0;
    let opponentScore = 0;
    let playerNumber = null;
    let playerName = '';
    let playerPhoto = '';

    // Mostrar selección de jugador al pulsar "Empezar"
    document.getElementById('start-btn').addEventListener('click', function() {
      document.getElementById('start-screen').classList.add('hidden');
      document.getElementById('player-select').classList.remove('hidden');
    });

    // Función para seleccionar jugador
    function selectPlayer(number) {
      playerNumber = number;
      document.getElementById('player-select').classList.add('hidden');
      document.getElementById('customization').classList.remove('hidden');
    }

    // Iniciar juego después de la personalización
    document.getElementById('start-game-btn').addEventListener('click', function() {
      playerName = document.getElementById('player-name').value;
      const playerColor = document.getElementById('player-color').value;

      // Si no subió ninguna foto, dar una predeterminada
      if (playerPhoto === '') {
        playerPhoto = 'https://via.placeholder.com/100'; // Placeholder en caso de no subir foto
      }

      alert(`¡El juego ha comenzado para el Jugador ${playerNumber}, ${playerName}!`);
      document.getElementById('player-name-display').textContent = playerName;
      document.getElementById('player-photo-preview').src = playerPhoto;
      document.getElementById('player-photo-preview').classList.remove('hidden');

      document.getElementById('customization').classList.add('hidden');
      document.getElementById('score-counter').classList.remove('hidden');

      // Estilo de color del jugador
      document.body.style.color = playerColor;
    });

    // Capturar la imagen que el jugador sube y almacenarla en base64
    document.getElementById('player-photo').addEventListener('change', function(event) {
      const reader = new FileReader();
      reader.onload = function(e) {
        playerPhoto = e.target.result; // Convertir la imagen en base64 para mostrarla más tarde
      };
      reader.readAsDataURL(event.target.files[0]);
    });

    // Función para agregar puntos
    document.getElementById('add-point-btn').addEventListener('click', function() {
      if (score < 10) {
        score++;
        document.getElementById('score').textContent = score;
      }
    });

    // Función para restar puntos
    document.getElementById('subtract-point-btn').addEventListener('click', function() {
      if (score > 0) {
        score--;
        document.getElementById('score').textContent = score;
      }
    });

    // Actualizar los puntos del oponente (aquí puedes adaptar el código según tu lógica)
    function updateOpponentScore(newScore) {
      opponentScore = newScore;
      document.getElementById('opponent-score').textContent = opponentScore;
    }

  </script>
</body>
</html>
