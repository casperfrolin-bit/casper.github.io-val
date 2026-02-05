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

:root {
  --btn-gap: 10px;
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
  animation: fall linear forwards;
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

/* === TEXT === */
.text-box {
  width: 700px;
  font-family: 'Noto Sans JP', sans-serif;
  font-size: 40px;
  text-align: center;
}

/* === KNAPPAR === */
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

.button-ja:hover {
  transform: scale(1.12);
}

.button-nej {
  background: #dcdcdc;
  position: absolute;
}

/* === KNAPP LAYOUT === */
.button-container {
  position: relative;
  margin-top: 20px;
  height: 80px;
}

/* ===== MODAL ===== */
.modal-bg {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.4);
  display: none;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal-box {
  background: #111;
  color: white;
  padding: 20px 30px;
  border-radius: 18px;
  font-family: 'Noto Sans JP', sans-serif;
  min-width: 280px;
  text-align: center;
}

.modal-btn {
  margin-top: 12px;
  padding: 6px 14px;
  border-radius: 16px;
  border: none;
  background: #e6f0ff;
  color: #1b5fd1;
  cursor: pointer;
  font-weight: bold;
}
</style>
</head>

<body>

<div class="hearts" id="hearts"></div>

<div class="center-box">
  <img src="https://thumbs.dreamstime.com/b/print-206284399.jpg" width="200">
  <div class="text-box">... vill du bli min valentine?</div>

  <div class="button-container">
    <button class="button button-ja" id="jaBtn">Ja</button>
    <button class="button button-nej" id="nejBtn">Nej</button>
  </div>
</div>

<div class="modal-bg" id="modal">
  <div class="modal-box">
    <div>OMGG JAAA 💖</div>
    <button class="modal-btn" onclick="closeModal()">OK</button>
  </div>
</div>

<script>
/* === HJÄRTAN === */
const hearts = document.getElementById("hearts");
function spawnHeart() {
  const h = document.createElement("div");
  h.className = "heart";
  h.textContent = "❤";
  h.style.left = Math.random() * 100 + "vw";
  h.style.fontSize = 12 + Math.random() * 18 + "px";
  h.style.animationDuration = 6 + Math.random() * 6 + "s";
  hearts.appendChild(h);
  setTimeout(() => h.remove(), 12000);
}
for (let i = 0; i < 30; i++) spawnHeart();
setInterval(spawnHeart, 200);

/* === NEJ-KNAPP: FLYTANDE & MJUK === */
const btn = document.getElementById("nejBtn");
const jaBtn = document.getElementById("jaBtn");

let x = jaBtn.offsetWidth + 20;
let y = 0;

let vx = 1.2;
let vy = 1.0;

const mouseForce = 0.06;
const drift = 0.02;
const padding = 20;
const cornerRadius = 160;

btn.style.left = x + "px";
btn.style.top = y + "px";

document.addEventListener("mousemove", e => {
  const r = btn.getBoundingClientRect();
  const cx = r.left + r.width / 2;
  const cy = r.top + r.height / 2;

  const dx = cx - e.clientX;
  const dy = cy - e.clientY;
  const dist = Math.hypot(dx, dy) || 1;

  vx += (dx / dist) * mouseForce;
  vy += (dy / dist) * mouseForce;
});

/* === ANIMATION LOOP === */
function animate() {
  const r = btn.getBoundingClientRect();

  x += vx;
  y += vy;

  vx += (Math.random() - 0.5) * drift;
  vy += (Math.random() - 0.5) * drift;

  const minX = padding;
  const minY = padding;
  const maxX = window.innerWidth - r.width - padding;
  const maxY = window.innerHeight - r.height - padding;

  if (x < minX || x > maxX) vx *= -1;
  if (y < minY || y > maxY) vy *= -1;

  /* ROUNDED CORNERS */
  const corners = [
    [minX + cornerRadius, minY + cornerRadius],
    [maxX - cornerRadius, minY + cornerRadius],
    [minX + cornerRadius, maxY - cornerRadius],
    [maxX - cornerRadius, maxY - cornerRadius]
  ];

  const bx = x + r.width / 2;
  const by = y + r.height / 2;

  for (const [cx, cy] of corners) {
    const dx = bx - cx;
    const dy = by - cy;
    const dist = Math.hypot(dx, dy);
    if (dist < cornerRadius) {
      const s = cornerRadius / (dist || 0.001);
      x = cx + dx * s - r.width / 2;
      y = cy + dy * s - r.height / 2;
      vx *= -0.9;
      vy *= -0.9;
    }
  }

  btn.style.left = x + "px";
  btn.style.top = y + "px";

  requestAnimationFrame(animate);
}

animate();

/* === MODAL === */
const modal = document.getElementById("modal");
jaBtn.addEventListener("click", () => modal.style.display = "flex");
function closeModal() { modal.style.display = "none"; }
</script>

</body>
</html>
