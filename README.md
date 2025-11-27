<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Christmas Tree – HTML/CSS/JS</title>
<style>
  /* -------- Global Styling -------- */
  body {
    margin: 0; padding: 0;
    background: #001f3f;           /* deep night blue */
    font-family: 'Courier New', monospace;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    color: #fff;
    flex-direction: column;
    overflow: hidden;
  }

  h1 {
    text-align: center;
    margin-bottom: 2rem;
    font-size: 2rem;
    text-shadow: 0 0 10px rgba(255,255,255,.7);
  }

  /* -------- Tree container -------- */
  .tree {
    position: relative;
    width: 200px;
    height: 280px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* -------- Tree branches -------- */
  .branch {
    width: 0; height: 0;
    border-left: 80px solid transparent;
    border-right: 80px solid transparent;
    border-bottom: 70px solid #006400;   /* dark green */
    position: relative;
    z-index: 2;
  }
  .branch:nth-child(1) { margin-top: 20px; }
  .branch:nth-child(2) { margin-top: -30px; }
  .branch:nth-child(3) { margin-top: -30px; }

  /* -------- Ornamental circles -------- */
  .ornament {
    position: absolute;
    border-radius: 50%;
    width: 12px; height: 12px;
    background: #ff0;
    animation: bounce 2s infinite ease-in-out;
    z-index: 3;
    box-shadow: 0 0 5px rgba(255,255,255,.8);
  }
  @keyframes bounce {
    0%,100% { transform: translateY(0); }
    50%     { transform: translateY(-5px); }
  }

  /* -------- Blinking lights -------- */
  .light {
    position: absolute;
    border-radius: 50%;
    width: 8px; height: 8px;
    background: #ffcc00;                /* warm yellow */
    z-index: 4;                          /* above ornaments */
    animation: flicker 1.5s infinite alternate;
    box-shadow: 0 0 6px rgba(255,204,0,.8);
  }
  @keyframes flicker {
    0%   { opacity: .2; filter: brightness(0.2); }
    20%  { opacity: 1;  filter: brightness(1);   }
    40%  { opacity: .8; filter: brightness(.8); }
    60%  { opacity: .2; filter: brightness(.2); }
    80%  { opacity: 1;  filter: brightness(1);   }
    100% { opacity: .2; filter: brightness(.2); }
  }

  /* -------- Trunk -------- */
  .trunk {
    width: 30px; height: 50px;
    background: #8B4513;   /* saddle brown */
    margin-top: 10px;
    border-radius: 5px;
    z-index: 1;
  }

  /* -------- Star -------- */
  .star {
    position: absolute; top: -15px;
    width: 0; height: 0;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-bottom: 15px solid #FFD700;   /* gold */
    z-index: 4;
    animation: twinkle 1.5s infinite alternate;
  }
  @keyframes twinkle { 0%{opacity:.7;} 100%{opacity:1;} }

  /* -------- Snowflakes -------- */
  .snowflake {
    position: absolute;
    color: #fff;
    top: -20px;
    animation: fall linear forwards;
    z-index: 5;
  }
  @keyframes fall { to{transform:translateY(105vh);} }

  /* -------- Ground snow effect -------- */
  .ground {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 50px;
    background: rgba(255,255,255,.8);
    z-index: 0;
  }
</style>
</head>
<body>
  <h1>🎄 Merry Christmas! 🎄</h1>

  <div class="tree">
    <div class="star"></div>
    <div class="branch"></div>
    <div class="branch"></div>
    <div class="branch"></div>
    <div class="trunk"></div>
  </div>

  <div class="ground"></div>

<script>
/* ---------- Create a handful of static blinking lights on every branch ---------- */
const tree = document.querySelector('.tree');
const branches = document.querySelectorAll('.branch');
const lightColors = ['#ff6e6e','#6ed6ff','#ff6eff','#6eff6e','#ffd700']; // a pop of colors

branches.forEach((branch, branchIndex) => {
  // 4‑6 lights per branch
  for (let i = 0; i < 4 + Math.floor(Math.random() * 3); i++) {
    const light = document.createElement('div');
    light.className = 'light';
    // Randomly position the light inside the triangular branch
    const left = 50 + (Math.random() - 0.5) * 100;
    const bottom = branchIndex * 70 + 20 + Math.random() * 40;
    light.style.left = `${left}px`;
    light.style.bottom = `${bottom}px`;
    light.style.background = lightColors[Math.floor(Math.random() * lightColors.length)];
    tree.appendChild(light);
  }
});

/* ---------- Ornament randomizer ---------- */
const colors = ['#ff0000','#00ff00','#0000ff','#ff69b4','#ffa500','#ffff00']; // variety

branches.forEach((branch, branchIndex) => {
  for (let i = 0; i < 6; i++) {            // 6 bouncing ornaments per branch
    const ornament = document.createElement('div');
    ornament.className = 'ornament';
    const left = 50 + (Math.random() - 0.5) * 100;
    const bottom = branchIndex * 70 + 20 + Math.random() * 40;
    ornament.style.left = `${left}px`;
    ornament.style.bottom = `${bottom}px`;
    ornament.style.background = colors[Math.floor(Math.random() * colors.length)];
    ornament.style.animationDelay = `${Math.random() * 2}s`;
    tree.appendChild(ornament);
  }
});

/* ---------- Snowflakes ---------- */
function createSnowflakes() {
  const snowflakes = ['❄', '❅', '❆'];
  const container = document.body;
  const snowflakeCount = 50;
  for (let i = 0; i < snowflakeCount; i++) {
    const snowflake = document.createElement('div');
    snowflake.classList.add('snowflake');
    snowflake.innerHTML = snowflakes[Math.floor(Math.random() * snowflakes.length)];
    snowflake.style.left = `${Math.random() * 100}vw`;
    snowflake.style.animationDuration = `${5 + Math.random() * 10}s`;
    snowflake.style.opacity = Math.random() * .7 + .3;
    snowflake.style.fontSize = `${Math.random() * 10 + 10}px`;
    container.appendChild(snowflake);
    setTimeout(() => snowflake.remove(), (5 + Math.random() * 10) * 1000);
  }
}
setInterval(createSnowflakes, 300);  // every 0.3s
createSnowflakes();

/* ---------- Background twinkles ---------- */
function addTwinkles() {
  const container = document.body;
  const twinkleCount = 20;
  for (let i = 0; i < twinkleCount; i++) {
    const twinkle = document.createElement('div');
    twinkle.style.position = 'absolute';
    twinkle.style.width = '2px';
    twinkle.style.height = '2px';
    twinkle.style.background = '#fff';
    twinkle.style.borderRadius = '50%';
    twinkle.style.left = `${Math.random() * 100}vw`;
    twinkle.style.top = `${Math.random() * 100}vh`;
    twinkle.style.animation = `twinkle ${1 + Math.random() * 2}s infinite alternate`;
    container.appendChild(twinkle);
  }
}
addTwinkles();
</script>
</body>
</html>
