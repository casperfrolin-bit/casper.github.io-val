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

/* ====== ÄNDRA MELLANRUMMET HÄR ====== */
:root {
  --btn-gap: 10px;   /* ändra denna siffra */
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
  transition: transform 0.15s ease-out;
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
}

/* layout så de står bredvid varandra */
.button-container {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--btn-gap);
  margin-top: 20px;
}

/* ===== MODAL-RUTA ===== */
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
  box-shadow: 0 4px 20px rgba(0,0,0,.4);
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

<!-- MODAL-RUTA -->
<div class="modal-bg" id="modal">
  <div class="modal-box">
    <div>OMGG JAAA 💖</div>
    <button class="modal-btn" onclick="closeModal()">OK</button>
  </div>
</div>

<script>
/* ===== OÄNDLIGA HJÄRTAN ===== */
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

/* ===== NEJ-KNAPP SOM FLYR ===== */
const btn = document.getElementById("nejBtn");
const jaBtn = document.getElementById("jaBtn");

const dangerRadius = 180;
const pushStep = 18;
const cornerRadius = 220;
const screenPadding = 15;

// Startposition: bredvid Ja-knappen
let x = jaBtn.offsetWidth + parseInt(getComputedStyle(document.documentElement).getPropertyValue('--btn-gap'));
let y = 0;

btn.style.position = 'absolute';
btn.style.left = x + 'px';
btn.style.top = y + 'px';

// Flytta knappen bara när musen kommer nära
document.addEventListener("mousemove", (e) => {
  const r = btn.getBoundingClientRect();
  const cx = r.left + r.width / 2;
  const cy = r.top + r.height / 2;

  const dx = e.clientX - cx;
  const dy = e.clientY - cy;
  const d = Math.hypot(dx, dy);

  if (d < dangerRadius) {
    // Flytta Nej-knappen
    x -= (dx / d) * pushStep;
    y -= (dy / d) * pushStep;

    // Begränsa inom skärmen
    const minX = screenPadding;
    const minY = screenPadding;
    const maxX = window.innerWidth - r.width - screenPadding;
    const maxY = window.innerHeight - r.height - screenPadding;

    x = Math.max(minX, Math.min(x, maxX));
    y = Math.max(minY, Math.min(y, maxY));

    // Hörnbegränsning
    const corners = [
      { cx: minX + cornerRadius, cy: minY + cornerRadius },
      { cx: maxX - cornerRadius, cy: minY + cornerRadius },
      { cx: minX + cornerRadius, cy: maxY - cornerRadius },
      { cx: maxX - cornerRadius, cy: maxY - cornerRadius }
    ];

    const bx = x + r.width / 2;
    const by = y + r.height / 2;

    for (const c of corners) {
      const vx = bx - c.cx;
      const vy = by - c.cy;
      const dist = Math.hypot(vx, vy);

      if (dist < cornerRadius) {
        const scale = cornerRadius / (dist || 0.1);
        x = c.cx + vx * scale - r.width / 2;
        y = c.cy + vy * scale - r.height / 2;
      }
    }

    btn.style.left = x + 'px';
    btn.style.top = y + 'px';
  }
});

/* ===== MODAL NÄR MAN TRYCKER JA ===== */
const modal = document.getElementById("modal");

jaBtn.addEventListener("click", () => {
  modal.style.display = "flex";
});

function closeModal() {
  modal.style.display = "none";
}
</script>

</body>
</html>
