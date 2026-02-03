
<html>
<head>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@700&display=swap" rel="stylesheet">

<style>
body {
  margin: 0;
  height: 100vh;
  background-color: #ffd1dc;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* === FALLANDE HJÄRTAN === */
.hearts {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: -1; /* 👈 bakom boxen */
}

.heart {
  position: absolute;
  color: #ff8fb1; /* 👈 rosa hjärtan */
  font-size: 20px;
  animation: fall linear infinite;
  opacity: 0.7;
}

@keyframes fall {
  0% {
    transform: translateY(-10vh);
  }
  100% {
    transform: translateY(110vh);
  }
}

/* === HJÄRTEXPLOSION === */
.explosion-heart {
  position: fixed;
  color: #ff4d6d;
  pointer-events: none;
  animation: explode 1s ease-out forwards;
}

@keyframes explode {
  0% {
    transform: translate(0, 0) scale(1);
    opacity: 1;
  }
  100% {
    transform: translate(var(--x), var(--y)) scale(0.5);
    opacity: 0;
  }
}

/* === CENTER BOX === */
.center-box {
  width: 900px;
  min-height: 550px;
  background: white;
  border-radius: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 20px;
  z-index: 1; /* 👈 över hjärtana */
}

.center-box img {
  width: 200px;
  height: 200px;
  border-radius: 10px;
  margin-bottom: 20px;
}

.text-box {
  width: 700px;
  padding: 10px;
  font-family: 'Noto Sans JP', sans-serif;
  font-weight: 700;
  font-size: 40px;
  color: #333;
  text-align: center;
}

.button {
  width: 100px;
  height: 60px;
  font-family: 'Noto Sans JP', sans-serif;
  font-weight: 700;
  font-size: 15px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}

.button-ja {
  background-color: #ff69b4;
  color: white;
}

.button-nej {
  background-color: #dcdcdc;
  color: #333;
  position: relative;
}

.button-container {
  display: flex;
  gap: 180px;
  margin-top: 20px;
}
</style>
</head>

<body>

<!-- FALLANDE HJÄRTAN -->
<div class="hearts" id="hearts"></div>

<div class="center-box">
  <img src="https://thumbs.dreamstime.com/b/print-206284399.jpg">
  <div class="text-box">
    ... vill du bli min valentine?
  </div>
  <div class="button-container">
    <button class="button button-ja">Ja</button>
    <button class="button button-nej">Nej</button>
  </div>
</div>

<script>
/* === FALLANDE HJÄRTAN SCRIPT === */
const heartsContainer = document.getElementById('hearts');
const heartCount = 30;

for (let i = 0; i < heartCount; i++) {
  const heart = document.createElement('div');
  heart.classList.add('heart');
  heart.innerHTML = '❤';

  heart.style.left = Math.random() * 100 + 'vw';
  heart.style.fontSize = Math.random() * 20 + 10 + 'px';
  heart.style.animationDuration = Math.random() * 6 + 10 + 's'; // 👈 långsammare
  heart.style.animationDelay = Math.random() * 5 + 's';

  heartsContainer.appendChild(heart);
}

/* === HJÄRTEXPLOSION VID JA === */
const jaButton = document.querySelector('.button-ja');

jaButton.addEventListener('click', () => {
  const rect = jaButton.getBoundingClientRect();
  const cx = rect.left + rect.width / 2;
  const cy = rect.top + rect.height / 2;

  for (let i = 0; i < 25; i++) {
    const heart = document.createElement('div');
    heart.classList.add('explosion-heart');
    heart.innerHTML = '❤';

    const angle = Math.random() * Math.PI * 2;
    const distance = Math.random() * 150 + 50;

    const x = Math.cos(angle) * distance;
    const y = Math.sin(angle) * distance;

    heart.style.left = cx + 'px';
    heart.style.top = cy + 'px';
    heart.style.fontSize = Math.random() * 15 + 15 + 'px';
    heart.style.setProperty('--x', x + 'px');
    heart.style.setProperty('--y', y + 'px');

    document.body.appendChild(heart);

    setTimeout(() => heart.remove(), 1000);
  }
});

/* === NEJ-KNAPPEN SOM FLYR === */
const nejButton = document.querySelector('.button-nej');
let x = 0;
let y = 0;
const dangerRadius = 150;

document.addEventListener('mousemove', (e) => {
  const rect = nejButton.getBoundingClientRect();
  const cx = rect.left + rect.width / 2;
  const cy = rect.top + rect.height / 2;

  const dx = e.clientX - cx;
  const dy = e.clientY - cy;
  const distance = Math.sqrt(dx * dx + dy * dy);

  if (distance < dangerRadius) {
    x -= (dx / distance) * 14;
    y -= (dy / distance) * 14;
  }

  const screenW = window.innerWidth;
  const screenH = window.innerHeight;

  if (rect.right < 0) x += screenW + rect.width;
  if (rect.left > screenW) x -= screenW + rect.width;
  if (rect.bottom < 0) y += screenH + rect.height;
  if (rect.top > screenH) y -= screenH + rect.height;

  nejButton.style.transform = `translate(${x}px, ${y}px)`;
});
</script>

</body>
</html>
