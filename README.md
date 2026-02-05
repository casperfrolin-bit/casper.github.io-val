
<html lang="sv">
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

.center-box {
  width: 900px;
  min-height: 550px;
  background: white;
  border-radius: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 20px;
}

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
  font-family: 'Noto Sans JP', sans-serif;
}

.button-ja {
  background: #ff69b4;
  color: white;
  cursor: pointer;
}

.button-nej {
  background: #dcdcdc;
  position: absolute;
  pointer-events: none; /* kan ej klickas */
}

.button-container {
  position: relative;
  margin-top: 20px;
  height: 80px;
}

/* === MODAL === */
.modal-bg {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.4);
  display: none;
  justify-content: center;
  align-items: center;
}
.modal-box {
  background: #111;
  color: white;
  padding: 20px 30px;
  border-radius: 18px;
  font-family: 'Noto Sans JP', sans-serif;
}
.modal-btn {
  margin-top: 12px;
  padding: 6px 14px;
  border-radius: 16px;
  border: none;
  cursor: pointer;
}
</style>
</head>

<body>

<div class="center-box">
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
/* ===============================
   NEJ-KNAPP = LÅDA SOM KNuffas
   =============================== */

const btn = document.getElementById("nejBtn");
const jaBtn = document.getElementById("jaBtn");

/* startposition */
let x = jaBtn.offsetWidth + 20;
let y = 0;

const pushRadius = 90;     // hur nära musen måste vara för att knuffa
const gap = 18;           // luft mellan mus och knapp
const pushSpeed = 1.0;    // hur lätt den glider
const padding = 20;

btn.style.left = x + "px";
btn.style.top = y + "px";

document.addEventListener("mousemove", e => {
  const r = btn.getBoundingClientRect();
  const cx = r.left + r.width / 2;
  const cy = r.top + r.height / 2;

  const dx = cx - e.clientX;
  const dy = cy - e.clientY;
  const dist = Math.hypot(dx, dy);

  /* musen är för långt bort → inget händer */
  if (dist > pushRadius || dist === 0) return;

  /* riktning bort från musen */
  const nx = dx / dist;
  const ny = dy / dist;

  /* knuff – som en fot mot en låda */
  x += nx * pushSpeed * (pushRadius - dist);
  y += ny * pushSpeed * (pushRadius - dist);

  clampToScreen(r);

  btn.style.left = x + "px";
  btn.style.top = y + "px";
});

/* === håll inom skärmen === */
function clampToScreen(r) {
  const minX = padding;
  const minY = padding;
  const maxX = window.innerWidth - r.width - padding;
  const maxY = window.innerHeight - r.height - padding;

  x = Math.max(minX, Math.min(x, maxX));
  y = Math.max(minY, Math.min(y, maxY));
}

/* === MODAL === */
const modal = document.getElementById("modal");
jaBtn.addEventListener("click", () => modal.style.display = "flex");
function closeModal() { modal.style.display = "none"; }
</script>

</body>
</html>

</html>
