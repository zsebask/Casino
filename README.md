<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Casino Simulator Avanzado</title>
<style>
  /* Reset y estilos básicos */
  * {
    margin: 0; padding: 0; box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    user-select: none;
  }
  body {
    background: radial-gradient(circle at center, #1a1a1a, #000);
    color: #f0d76a;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  header {
    width: 100%;
    background: #3a1f00;
    padding: 1rem;
    text-align: center;
    box-shadow: 0 0 15px #f0d76a;
  }
  header h1 {
    font-size: 2.5rem;
    text-shadow: 0 0 10px #f0d76a;
  }
  nav {
    margin-top: 1rem;
  }
  .nav-btn {
    background: #5a3300;
    border: none;
    color: #f0d76a;
    padding: 0.6rem 1.2rem;
    margin: 0 0.5rem;
    font-weight: bold;
    cursor: pointer;
    border-radius: 5px;
    box-shadow: 0 0 8px #d4b94f;
    transition: background 0.3s ease;
  }
  .nav-btn:hover,
  .nav-btn.active {
    background: #f0d76a;
    color: #3a1f00;
    box-shadow: 0 0 15px #f0d76a;
  }
  main {
    margin: 2rem auto;
    width: 90%;
    max-width: 700px;
    background: #2c1b00;
    border-radius: 15px;
    padding: 2rem;
    box-shadow: 0 0 20px #f0d76a;
    min-height: 480px;
    color: #fffbe0;
  }
  .game-section {
    display: none;
  }
  .game-section.active {
    display: block;
  }
  h2 {
    text-align: center;
    margin-bottom: 1rem;
    text-shadow: 0 0 8px #f0d76a;
  }
  .roulette-container {
    display: flex;
    justify-content: center;
    margin-bottom: 1rem;
  }
  .spin-btn {
    display: block;
    margin: 0 auto 1rem auto;
    padding: 0.8rem 1.5rem;
    font-size: 1.1rem;
    background: #f0d76a;
    border: none;
    border-radius: 8px;
    color: #3a1f00;
    font-weight: bold;
    cursor: pointer;
    box-shadow: 0 0 15px #f0d76a;
    transition: background 0.3s ease;
  }
  .spin-btn:hover {
    background: #d4b94f;
  }
  button {
    cursor: pointer;
  }
  /* Cartas */
  #cards-container {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin: 1rem 0;
  }
  .card {
    width: 80px;
    height: 120px;
    background: #5a3300;
    border-radius: 10px;
    box-shadow: inset 0 0 10px #d4b94f;
    color: #f0d76a;
    font-weight: bold;
    font-size: 2rem;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    user-select: none;
    transition: background 0.3s ease;
  }
  .card:hover {
    background: #f0d76a;
    color: #3a1f00;
    box-shadow: 0 0 15px #f0d76a;
  }
  .card.selected {
    background: #d4b94f;
    color: #3a1f00;
    box-shadow: 0 0 20px #f0d76a;
    cursor: default;
  }
  /* Minas */
  #mines-grid {
    display: grid;
    grid-template-columns: repeat(5, 50px);
    grid-gap: 10px;
    justify-content: center;
    margin: 1rem 0;
  }
  .mine-cell {
    width: 50px;
    height: 50px;
    background: #5a3300;
    border-radius: 8px;
    box-shadow: inset 0 0 8px #d4b94f;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    font-size: 1.2rem;
    color: transparent;
    user-select: none;
    transition: background 0.3s ease, color 0.3s ease;
  }
  .mine-cell.revealed {
    background: #f0d76a;
    color: #3a1f00;
    cursor: default;
    box-shadow: 0 0 10px #f0d76a;
  }
  .mine-cell.mine {
    background: #a83232;
    color: #fff;
    box-shadow: 0 0 15px #ff4c4c;
  }
  #mines-message {
    text-align: center;
    font-weight: bold;
    margin-top: 0.5rem;
    min-height: 1.5rem;
  }
  #mines-options {
    text-align: center;
    margin-bottom: 1rem;
  }
  #mines-options label {
    margin-right: 10px;
  }
  /* Todo o nada */
  #allornone-result {
    text-align: center;
    margin-top: 1rem;
    font-weight: bold;
    min-height: 1.5rem;
  }
  footer {
    margin-top: auto;
    padding: 1rem;
    width: 100%;
    text-align: center;
    background: #3a1f00;
    color: #f0d76a;
    box-shadow: 0 0 15px #f0d76a;
    font-size: 0.9rem;
  }
</style>
</head>
<body>
<header>
  <h1>🎰 Casino Simulator Avanzado 🎲</h1>
  <nav>
    <button id="btn-roulette" class="nav-btn active">Ruleta (x2 / x5)</button>
    <button id="btn-cards" class="nav-btn">Cartas (x3)</button>
    <button id="btn-mines" class="nav-btn">Minas (x1.25 - x2)</button>
    <button id="btn-allornone" class="nav-btn">Todo o Nada</button>
  </nav>
</header>

<main>
  <!-- Ruleta -->
  <section id="roulette-section" class="game-section active">
    <h2>Ruleta - Multiplicador x2 (40%) y x5 (en medio de perder)</h2>
    <div class="roulette-container">
      <canvas id="roulette-canvas" width="300" height="300"></canvas>
    </div>
    <button id="spin-btn" class="spin-btn">Girar Ruleta</button>
    <p id="roulette-result"></p>
  </section>

  <!-- Cartas -->
  <section id="cards-section" class="game-section">
    <h2>Juego de Cartas - Multiplicador x3</h2>
    <p>Elige una carta entre 4. Si aciertas, ganas x3.</p>
    <div id="cards-container">
      <div class="card" data-card="A">A</div>
      <div class="card" data-card="K">K</div>
      <div class="card" data-card="Q">Q</div>
      <div class="card" data-card="J">J</div>
    </div>
    <p id="cards-result"></p>
    <button id="cards-reset-btn" style="display:none; margin: 1rem auto; padding: 0.5rem 1rem; background:#f0d76a; border:none; border-radius:6px; font-weight:bold; color:#3a1f00; cursor:pointer;">Jugar de nuevo</button>
  </section>

  <!-- Minas -->
  <section id="mines-section" class="game-section">
    <h2>Juego de Minas - Multiplicador variable según minas evitadas</h2>
    <div id="mines-options">
      <label for="mines-count">Elige número de minas (3 a 8): </label>
      <input type="number" id="mines-count" min="3" max="8" value="3" />
      <button id="mines-set-btn" class="spin-btn" style="padding: 0.3rem 0.8rem; font-size: 1rem;">Establecer</button>
    </div>
    <div id="mines-grid"></div>
    <p id="mines-message"></p>
    <p id="mines-multiplier" style="text-align:center; font-weight:bold; margin-top:0.5rem;"></p>
    <button id="mines-reset-btn" class="spin-btn" style="margin-top: 1rem;">Reiniciar Juego</button>
  </section>

  <!-- Todo o Nada -->
  <section id="allornone-section" class="game-section">
    <h2>Zona Todo o Nada - Apuesta todo y juega aleatorio</h2>
    <p>Elige jugar Ruleta, Cartas o Minas al azar. Ganas o pierdes todo.</p>
    <button id="allornone-btn" class="spin-btn">Jugar Todo o Nada</button>
    <p id="allornone-result"></p>
  </section>
</main>

<footer>
  <p>Proyecto personal - Casino Simulator Avanzado &copy; 2025</p>
</footer>

<script>
  // Navegación entre juegos
  const btnRoulette = document.getElementById('btn-roulette');
  const btnCards = document.getElementById('btn-cards');
  const btnMines = document.getElementById('btn-mines');
  const btnAllOrNone = document.getElementById('btn-allornone');

  const sectionRoulette = document.getElementById('roulette-section');
  const sectionCards = document.getElementById('cards-section');
  const sectionMines = document.getElementById('mines-section');
  const sectionAllOrNone = document.getElementById('allornone-section');

  const navButtons = [btnRoulette, btnCards, btnMines, btnAllOrNone];
  const sections = [sectionRoulette, sectionCards, sectionMines, sectionAllOrNone];

  function activateSection(index) {
    sections.forEach((sec, i) => {
      sec.classList.toggle('active', i === index);
      navButtons[i].classList.toggle('active', i === index);
    });
  }

  btnRoulette.addEventListener('click', () => activateSection(0));
  btnCards.addEventListener('click', () => activateSection(1));
  btnMines.addEventListener('click', () => activateSection(2));
  btnAllOrNone.addEventListener('click', () => activateSection(3));
</script>

<script>
  // Ruleta con 60% perder, 40% ganar x2, y un x5 en medio de perder
  const canvas = document.getElementById('roulette-canvas');
  const ctx = canvas.getContext('2d');
  const spinBtn = document.getElementById('spin-btn');
  const resultText = document.getElementById('roulette-result');

  // 10 segmentos: 6 perder, 1 x5 en medio, 3 x2
  // Orden: perder(3), x5, perder(3), x2(3)
  const segments = [
    { color: '#a83232', label: 'Perder', multiplier: 0 },
    { color: '#a83232', label: 'Perder', multiplier: 0 },
    { color: '#a83232', label: 'Perder', multiplier: 0 },
    { color: '#bfa134', label: 'x5', multiplier: 5 },  // mitad de perder
    { color: '#a83232', label: 'Perder', multiplier: 0 },
    { color: '#a83232', label: 'Perder', multiplier: 0 },
    { color: '#a83232', label: 'Perder', multiplier: 0 },
    { color: '#f0d76a', label: 'x2', multiplier: 2 },
    { color: '#f0d76a', label: 'x2', multiplier: 2 },
    { color: '#f0d76a', label: 'x2', multiplier: 2 },
  ];

  const centerX = canvas.width / 2;
  const centerY = canvas.height / 2;
  const radius = 140;

  let startAngle = 0;
  let spinTimeout = null;
  let spinAngleStart = 0;
  let spinTime = 0;
  let spinTimeTotal = 0;

  function drawSegment(index, startAngle, arc) {
    const segment = segments[index];
    ctx.beginPath();
    ctx.fillStyle = segment.color;
    ctx.moveTo(centerX, centerY);
    ctx.arc(centerX, centerY, radius, startAngle, startAngle + arc, false);
    ctx.lineTo(centerX, centerY);
    ctx.fill();

    // Texto
    ctx.save();
    ctx.fillStyle = segment.multiplier === 0 ? '#fff' : '#3a1f00';
    ctx.font = 'bold 18px Arial';
    ctx.translate(centerX, centerY);
    ctx.rotate(startAngle + arc / 2);
    ctx.textAlign = 'right';
    ctx.fillText(segment.label, radius - 10, 10);
    ctx.restore();
  }

  function drawRoulette() {
    const arc = (2 * Math.PI) / segments.length;
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    for (let i = 0; i < segments.length; i++) {
      drawSegment(i, startAngle + i * arc, arc);
    }

    // Dibuja la flecha
    ctx.fillStyle = '#ff0000';
    ctx.beginPath();
    ctx.moveTo(centerX - 10, centerY - radius - 10);
    ctx.lineTo(centerX + 10, centerY - radius - 10);
    ctx.lineTo(centerX, centerY - radius + 10);
    ctx.closePath();
    ctx.fill();
  }

  function spin() {
    spinAngleStart = Math.random() * 10 + 10;
    spinTime = 0;
    spinTimeTotal = Math.random() * 3000 + 4000;
    rotateWheel();
  }

  function rotateWheel() {
    spinTime += 30;
    if (spinTime >= spinTimeTotal) {
      stopRotateWheel();
      return;
    }
    const spinAngle =
      spinAngleStart - easeOut(spinTime, 0, spinAngleStart, spinTimeTotal);
    startAngle += (spinAngle * Math.PI) / 180;
    drawRoulette();
    spinTimeout = setTimeout(rotateWheel, 30);
  }

  function stopRotateWheel() {
    clearTimeout(spinTimeout);
    const arc = (2 * Math.PI) / segments.length;
    const degrees = (startAngle * 180) / Math.PI + 90;
    const index =
      Math.floor(
        segments.length -
          ((degrees / (360 / segments.length)) % segments.length)
      ) % segments.length;
    const segment = segments[index];
    if (segment.multiplier === 0) {
      resultText.textContent = `¡Perdiste! Mejor suerte la próxima vez.`;
      resultText.style.color = '#ff4c4c';
    } else {
      resultText.textContent = `¡Ganaste un multiplicador de ${segment.label}! 🎉`;
      resultText.style.color = '#f0d76a';
    }
  }

  function easeOut(t, b, c, d) {
    const ts = (t /= d) * t;
    const tc = ts * t;
    return b + c * (tc + -3 * ts + 3 * t);
  }

  drawRoulette();
  spinBtn.addEventListener('click', () => {
    resultText.textContent = 'Girando...';
    resultText.style.color = '#f0d76a';
    spin();
  });
</script>

<script>
  // Juego de cartas - Multiplicador x3
  const cardsContainer = document.getElementById('cards-container');
  const cardsResult = document.getElementById('cards-result');
  const cardsResetBtn = document.getElementById('cards-reset-btn');

  const cards = ['A', 'K', 'Q', 'J'];
  let cardChosen = null;
  let winningCard = null;
  let gameActive = true;

  function resetCards() {
    cardChosen = null;
    winningCard = cards[Math.floor(Math.random() * cards.length)];
    gameActive = true;
    cardsResult.textContent = '';
    cardsResetBtn.style.display = 'none';
    Array.from(cardsContainer.children).forEach(card => {
      card.classList.remove('selected');
      card.style.pointerEvents = 'auto';
    });
    //console.log('Carta ganadora:', winningCard);
  }

  cardsContainer.addEventListener('click', e => {
    if (!gameActive) return;
    const target = e.target;
    if (!target.classList.contains('card')) return;

    cardChosen = target.dataset.card;
    gameActive = false;

    Array.from(cardsContainer.children).forEach(card => {
      card.classList.remove('selected');
      card.style.pointerEvents = 'none';
    });
    target.classList.add('selected');

    if (cardChosen === winningCard) {
      cardsResult.textContent = `¡Ganaste! La carta correcta era ${winningCard}. Multiplicador x3 🎉`;
      cardsResult.style.color = '#f0d76a';
    } else {
      cardsResult.textContent = `Perdiste. La carta correcta era ${winningCard}. Intenta de nuevo.`;
      cardsResult.style.color = '#ff4c4c';
    }
    cardsResetBtn.style.display = 'inline-block';
  });

  cardsResetBtn.addEventListener('click', resetCards);

  resetCards();
</script>

<script>
  // Juego de minas 5x5 con minas variables (3 a 8)
  const minesGrid = document.getElementById('mines-grid');
  const minesMessage = document.getElementById('mines-message');
  const minesMultiplierDisplay = document.getElementById('mines-multiplier');
  const minesResetBtn = document.getElementById('mines-reset-btn');
  const minesCountInput = document.getElementById('mines-count');
  const minesSetBtn = document.getElementById('mines-set-btn');

  const gridSize = 5;
  const totalCells = gridSize * gridSize;

  // Multiplicadores base según minas (3 a 8)
  const baseMultipliers = {
    3: 1.25,
    4: 1.40,
    5: 1.55,
    6: 1.70,
    7: 1.90,
    8: 2.00
  };

  let minesPositions = [];
  let revealedCount = 0;
  let gameOver = false;
  let currentMinesCount = 3;

  function initMines() {
    minesGrid.innerHTML = '';
    minesPositions = [];
    revealedCount = 0;
    gameOver = false;
    minesMessage.textContent = 'Selecciona una celda para comenzar.';
    minesMessage.style.color = '#f0d76a';

    // Validar minas
    if (currentMinesCount < 3) currentMinesCount = 3;
    if (currentMinesCount > 8) currentMinesCount = 8;

    // Generar minas aleatorias
    while (minesPositions.length < currentMinesCount) {
      const pos = Math.floor(Math.random() * totalCells);
      if (!minesPositions.includes(pos)) {
        minesPositions.push(pos);
      }
    }

    for (let i = 0; i < totalCells; i++) {
      const cell = document.createElement('div');
      cell.classList.add('mine-cell');
      cell.dataset.index = i;
      cell.addEventListener('click', onCellClick);
      minesGrid.appendChild(cell);
    }
    updateMultiplierDisplay();
  }

  function onCellClick(e) {
    if (gameOver) return;
    const cell = e.currentTarget;
    const index = parseInt(cell.dataset.index);

    if (cell.classList.contains('revealed')) return;

    if (minesPositions.includes(index)) {
      cell.classList.add('mine', 'revealed');
      gameOver = true;
      revealAllMines();
      minesMessage.textContent = '¡Perdiste! Encontraste una mina 💥';
      minesMessage.style.color = '#ff4c4c';
      minesMultiplierDisplay.textContent = '';
    } else {
      cell.classList.add('revealed');
      revealedCount++;
      const multiplier = calculateMultiplier();
      minesMessage.textContent = `¡Bien! Celdas seguras: ${revealedCount}`;
      minesMessage.style.color = '#f0d76a';
      minesMultiplierDisplay.textContent = `Multiplicador actual: x${multiplier.toFixed(2)}`;
      if (revealedCount === totalCells - currentMinesCount) {
        gameOver = true;
        minesMessage.textContent = `¡Ganaste! Evitaste todas las minas 🎉`;
        minesMultiplierDisplay.textContent += ` - Ganancia final: x${multiplier.toFixed(2)}`;
        revealAllMines();
      }
    }
  }

  function revealAllMines() {
    const cells = minesGrid.querySelectorAll('.mine-cell');
    cells.forEach((cell, i) => {
      if (minesPositions.includes(i)) {
        cell.classList.add('mine', 'revealed');
      }
    });
  }

  function calculateMultiplier() {
    // Base + 0.15 por mina evitada
    const base = baseMultipliers[currentMinesCount];
    const avoidMines = revealedCount;
    return base + avoidMines * 0.15;
  }

  function updateMultiplierDisplay() {
    const base = baseMultipliers[currentMinesCount];
    minesMultiplierDisplay.textContent = `Multiplicador base: x${base.toFixed(2)} + 0.15 por mina evitada`;
  }

  minesResetBtn.addEventListener('click', initMines);

  minesSetBtn.addEventListener('click', () => {
    const val = parseInt(minesCountInput.value);
    if (val >= 3 && val <= 8) {
      currentMinesCount = val;
      initMines();
    } else {
      alert('Por favor ingresa un número entre 3 y 8 para minas.');
    }
  });

  initMines();
</script>

<script>
  // Zona Todo o Nada - juego aleatorio
  const allOrNoneBtn = document.getElementById('allornone-btn');
  const allOrNoneResult = document.getElementById('allornone-result');

  allOrNoneBtn.addEventListener('click', () => {
    allOrNoneResult.textContent = 'Jugando...';
    allOrNoneResult.style.color = '#f0d76a';

    // Elegir juego aleatorio: 0=Ruleta,1=Cartas,2=Minas
    const game = Math.floor(Math.random() * 3);

    if (game === 0) {
      // Ruleta todo o nada: 60% perder, 40% ganar x2
      const rand = Math.random();
      if (rand < 0.6) {
        allOrNoneResult.textContent = 'Ruleta: ¡Perdiste todo! 💥';
        allOrNoneResult.style.color = '#ff4c4c';
      } else {
        allOrNoneResult.textContent = 'Ruleta: ¡Ganaste x2! 🎉';
        allOrNoneResult.style.color = '#f0d76a';
      }
    } else if (game === 1) {
      // Cartas todo o nada: 1 de 4 gana x3
      const winningCard = ['A', 'K', 'Q', 'J'][Math.floor(Math.random() * 4)];
      const userCard = ['A', 'K', 'Q', 'J'][Math.floor(Math.random() * 4)];
      if (userCard === winningCard) {
        allOrNoneResult.textContent = `Cartas: Elegiste ${userCard} y ganó ${winningCard}. ¡Ganaste x3! 🎉`;
        allOrNoneResult.style.color = '#f0d76a';
      } else {
        allOrNoneResult.textContent = `Cartas: Elegiste ${userCard} y ganó ${winningCard}. Perdiste todo. 💥`;
        allOrNoneResult.style.color = '#ff4c4c';
      }
    } else {
      // Minas todo o nada: 5 minas en 5x5, gana si evita todas
      const minesCount = 5;
      const totalCells = 25;
      let minesPositions = [];
      while (minesPositions.length < minesCount) {
        const pos = Math.floor(Math.random() * totalCells);
        if (!minesPositions.includes(pos)) minesPositions.push(pos);
      }
      // Simular evitar minas con 70% chance de éxito
      const success = Math.random() < 0.7;
      if (success) {
        allOrNoneResult.textContent = `Minas: Evitaste todas las minas. ¡Ganaste x1.55! 🎉`;
        allOrNoneResult.style.color = '#f0d76a';
      } else {
        allOrNoneResult.textContent = `Minas: Encontraste una mina. Perdiste todo. 💥`;
        allOrNoneResult.style.color = '#ff4c4c';
      }
    }
  });
</script>

</body>
</html>
