
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
  font-family: 'Noto Sans JP', sans-serif;
  font-size: 40px;
  text-align: center;
}

/* === KNAPPAR === */
.button-container {
  display: flex;
  gap: 260px;
  margin-top: 30px;
  transform: translateX(-80px);
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
  color: #333;
  position: fixed; /* 👈 alltid fixed */
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
    <button class="button button-nej" id="nej">Nej</button>
  </div>
</div>

<script>
/* === HJÄRTAN (MITTEN) === */
const hearts = document.getElementById("hearts");
for (let i = 0; i < 45; i++) {
  const h = document.createElement("div");
  h.className = "heart";
  h.textContent = "❤";
  h.style.left = Math.random() * 100 + "vw";
  h.style.top = 45 + (Math.random() * 20 - 10) + "vh";
  h.style.fontSize = 12 + Math.random() * 18 + "px";
  h.style.animationDuration = 10 + Math.random() * 10 + "s";
  h.style.animationDelay = (-Math.random() * 10) + "s";
  hearts.appendChild(h);
}

/* === NEJ-KNAPP (BOMBSÄKER) === */
const btn = document.getElementById("nej");
const dangerRadius = 150;
const cornerRadius = 200;

// 🔒 MANUELL STARTPOSITION (ALLTID SYNLIG)
let x = window.innerWidth / 2 + 120;
let y = window.innerHeight / 2 + 40;

btn.style.left = x + "px";
btn.style.top = y + "px";

document.addEventListener("mousemove", (e) => {
  const rect = btn.getBoundingClientRect();

  const cx = rect.left + rect.width / 2;
  const cy = rect.top + rect.height / 2;

  const dx = e.clientX - cx;
  const dy = e.clientY - cy;
  const d = Math.hypot(dx, dy);

  if (d < dangerRadius) {
    x -= (dx / d) * 14;
    y -= (dy / d) * 14;
  }

  const minX = 0;
  const minY = 0;
  const maxX = window.innerWidth - rect.width;
  const maxY = window.innerHeight - rect.height;

  // Rektangel
  x = Math.max(minX, Math.min(x, maxX));
  y = Math.max(minY, Math.min(y, maxY));

  // Rundade hörn
  const corners = [
    { cx: cornerRadius, cy: cornerRadius },
    { cx: maxX - cornerRadius, cy: cornerRadius },
    { cx: cornerRadius, cy: maxY - cornerRadius },
    { cx: maxX - cornerRadius, cy: maxY - cornerRadius }
  ];

  for (const c of corners) {
    const vx = x + rect.width / 2 - c.cx;
    const vy = y + rect.height / 2 - c.cy;
    const dist = Math.hypot(vx, vy);

    if (dist > cornerRadius) {
      const scale = cornerRadius / dist;
      x = c.cx + vx * scale - rect.width / 2;
      y = c.cy + vy * scale - rect.height / 2;
    }
  }

  btn.style.left = x + "px";
  btn.style.top = y + "px";
});
</script>

</body>
</html>
