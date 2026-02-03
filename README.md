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
  z-index: -1;
}

.heart {
  position: absolute;
  color: #ff8fb1;
  font-size: 20px;
  animation: fall linear infinite;
  opacity: 0.7;
}

@keyframes fall {
  0% { transform: translateY(-10vh); }
  100% { transform: translateY(110vh); }
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
  z-index: 1;
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
  position: fixed;
}

.button-container {
  display: flex;
  gap: 180px;
  margin-top: 20px;
}
</style>
</head>

<body>

<div class="hearts" id="hearts"></div>

<div class="center-box">
  <img src="https://thumbs.dreamstime.com/b/print-206284399.jpg">
  <div class="text-box">... vill du bli min valentine?</div>
  <div class="button-container">
    <button class="button button-ja">Ja</button>
    <button class="button button-nej">Nej</button>
  </div>
</div>

<script>
/* === FALLANDE HJÄRTAN === */
const heartsContainer = document.getElementById('hearts');
const heartCount = 45;

for (let i = 0; i < heartCount; i++) {
  const heart = document.createElement('div');
  heart.classList.add('heart');
  heart.innerHTML = '❤';

  heart.style.left = Math.random() * 100 + 'vw';
  heart.style.top = Math.random() * 40 + 'vh';
  heart.style.fontSize = Math.random() * 18 + 12 + 'px';

  const duration = Math.random() * 6 + 12;
  heart.style.animationDuration = duration + 's';
  heart.style.animationDelay = (-Math.random() * duration) + 's';

  heartsContainer.appendChild(heart);
}

/* === NEJ-KNAPPEN SOM FLYR I CIRKEL === */
const nejButton = document.querySelector('.button-nej');
const dangerRadius = 150;

// Startposition
let posX = window.innerWidth / 2 + 100;
let posY = window.innerHeight / 2 + 100;

nejButton.style.left = posX + 'px';
nejButton.style.top = posY + 'px';

document.addEventListener('mousemove', (e) => {
  const rect = nejButton.getBoundingClientRect();

  const btnCenterX = rect.left + rect.width / 2;
  const btnCenterY = rect.top + rect.height / 2;

  const dx = e.clientX - btnCenterX;
  const dy = e.clientY - btnCenterY;
  const distance = Math.sqrt(dx * dx + dy * dy);

  // Fly undan musen
  if (distance < dangerRadius) {
    posX -= (dx / distance) * 14;
    posY -= (dy / distance) * 14;
  }

  /* === CIRKULÄR BEGRÄNSNING === */
  const screenCenterX = window.innerWidth / 2;
  const screenCenterY = window.innerHeight / 2;

  const radius =
    Math.min(window.innerWidth, window.innerHeight) / 2
    - Math.max(rect.width, rect.height)
    - 20;

  const vx = posX + rect.width / 2 - screenCenterX;
  const vy = posY + rect.height / 2 - screenCenterY;
  const distFromCenter = Math.sqrt(vx * vx + vy * vy);

  if (distFromCenter > radius) {
    const scale = radius / distFromCenter;
    posX = screenCenterX + vx * scale - rect.width / 2;
    posY = screenCenterY + vy * scale - rect.height / 2;
  }

  nejButton.style.left = posX + 'px';
  nejButton.style.top = posY + 'px';
});
</script>

</body>
</html>
