<!DOCTYPE html>
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
  inset: 0;
  pointer-events: none;
  z-index: -1;
}

.heart {
  position: absolute;
  color: #ff8fb1;
  animation: fall linear infinite;
  opacity: 0.8;
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

.text-box {
  width: 700px;
  font-family: 'Noto Sans JP', sans-serif;
  font-size: 40px;
  text-align: center;
}

.button-container {
  display: flex;
  gap: 180px;
  margin-top: 20px;
}

.button {
  width: 100px;
  height: 60px;
  border-radius: 50px;
  border: none;
  cursor: pointer;
  font-family: 'Noto Sans JP', sans-serif;
}

.button-ja {
  background: #ff69b4;
  color: white;
}

.button-nej {
  background: #dcdcdc;
  pointer-events: none; /* omöjlig att klicka */
}
</style>
</head>

<body>

<div class="hearts" id="hearts"></div>

<div class="center-box">
  <img src="https://thumbs.dreamstime.com/b/print-206284399.jpg" width="200">
  <div class="text-box">... vill du bli min valentine?</div>

  <div class="button-container">
    <button class="button button-ja">Ja</button>
    <button class="button button-nej">Nej</button>
  </div>
</div>

<script>
/* === MASSOR AV HJÄRTAN === */
const hearts = document.getElementById("hearts");

function spawnHeart() {
  const h = document.createElement("div");
  h.className = "heart";
  h.textContent = "❤";
  h.style.left = Math.random() * 100 + "vw";
  h.style.fontSize = 12 + Math.random() * 24 + "px";
  h.style.animationDuration = 6 + Math.random() * 8 + "s";
  hearts.appendChild(h);
  setTimeout(() => h.remove(), 15000);
}

for (let i = 0; i < 80; i++) spawnHeart();
setInterval(() => {
  for (let i = 0; i < 5; i++) spawnHeart();
}, 500);

/* === NEJ-KNAPP === */
const btn = document.querySelector(".button-nej");

let x = 0;
let y = 0;
let initialized = false;
let lastMouseX = 0;
let lastMouseY = 0;

document.addEventListener("mousemove", (e) => {
  const r = btn.getBoundingClientRect();

  /* Initiera exakt där knappen redan är */
  if (!initialized) {
    x = r.left;
    y = r.top;
    btn.style.position = "fixed";
    initialized = true;
  }

  const cx = x + r.width / 2;
  const cy = y + r.height / 2;

  const dx = e.clientX - cx;
  const dy = e.clientY - cy;
  const d = Math.hypot(dx, dy);

  const mouseSpeed = Math.hypot(
    e.clientX - lastMouseX,
    e.clientY - lastMouseY
  ) || 1;

  if (d < 200) {
    x -= (dx / d) * mouseSpeed * 1.3;
    y -= (dy / d) * mouseSpeed * 1.3;
  }

  lastMouseX = e.clientX;
  lastMouseY = e.clientY;

  /* === CIRKULÄR GRÄNS (inga hörn) === */
  const centerX = window.innerWidth / 2;
  const centerY = window.innerHeight / 2;
  const radius = Math.min(window.innerWidth, window.innerHeight) / 2 - 60;

  const vx = x - centerX;
  const vy = y - centerY;
  const dist = Math.hypot(vx, vy);

  if (dist > radius) {
    const scale = radius / dist;
    x = centerX + vx * scale;
    y = centerY + vy * scale;
  }

  btn.style.left = x + "px";
  btn.style.top = y + "px";
});
</script>

</body>
</html>
