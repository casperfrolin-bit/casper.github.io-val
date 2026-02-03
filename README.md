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
  position: relative; /* 👈 viktigt */
  z-index: 1;
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
  justify-content: center;
  gap: 200px;        /* 👈 stort mellanrum */
  margin-top: 50px;  /* 👈 under texten */
}

.button {
  width: 120px;
  height: 65px;
  border-radius: 50px;
  border: none;
  cursor: pointer;
  font-family: 'Noto Sans JP', sans-serif;
  font-size: 22px;
}

.button-ja {
  background: #ff69b4;
  color: white;
}

.button-nej {
  background: #dcdcdc;
  position: absolute; /* 👈 inte fixed längre */
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
/* === FALLANDE HJÄRTAN === */
const hearts = document.getElementById("hearts");
for (let i = 0; i < 40; i++) {
  const h = document.createElement("div");
  h.className = "heart";
  h.textContent = "❤";
  h.style.left = Math.random() * 100 + "vw";
  h.style.fontSize = 12 + Math.random() * 18 + "px";
  h.style.animationDuration = 10 + Math.random() * 10 + "s";
  hearts.appendChild(h);
}

/* === NEJ-KNAPP SOM FLYR === */
const btn = document.querySelector(".button-nej");
const dangerRadius = 150;

let x = 520;
let y = 380;

btn.style.left = x + "px";
btn.style.top = y + "px";

document.addEventListener("mousemove", (e) => {
  const r = btn.getBoundingClientRect();
  const cx = r.left + r.width / 2;
  const cy = r.top + r.height / 2;

  const dx = e.clientX - cx;
  const dy = e.clientY - cy;
  const d = Math.hypot(dx, dy);

  if (d < dangerRadius) {
    x -= (dx / d) * 14;
    y -= (dy / d) * 14;
  }

  const box = document.querySelector(".center-box").getBoundingClientRect();

  x = Math.max(box.left, Math.min(x, box.right - r.width));
  y = Math.max(box.top, Math.min(y, box.bottom - r.height));

  btn.style.left = x + "px";
  btn.style.top = y + "px";
});
</script>

</body>
</html>
