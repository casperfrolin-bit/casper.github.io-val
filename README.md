<html>
<head>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@700&display=swap" rel="stylesheet">

<style>
body {
  margin: 0;
  height: 100vh;
  background-color: #ffd1dc;
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
  margin-top: 40px;
}
</style>
</head>

<body>

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
const nejButton = document.querySelector('.button-nej');
const jaButton = document.querySelector('.button-ja');

let x = 0;
let y = 0;
const dangerRadius = 150;

// 🔹 Minnesflagga – så Ja bara växer EN gång
let hasMoved = false;

document.addEventListener('mousemove', (e) => {
  const rect = nejButton.getBoundingClientRect();

  const cx = rect.left + rect.width / 2;
  const cy = rect.top + rect.height / 2;

  const dx = e.clientX - cx;
  const dy = e.clientY - cy;

  const distance = Math.sqrt(dx * dx + dy * dy);

  if (distance < dangerRadius) {
    const nx = dx / distance;
    const ny = dy / distance;

    x -= nx * 14;
    y -= ny * 14;

    // 🔹 Gör Ja större – men bara första gången
    if (!hasMoved) {
      jaButton.style.transform = "scale(1.6)";
      jaButton.style.transition = "transform 0.2s ease";
      hasMoved = true;
    }
  }

  // === WRAP AROUND SKÄRMEN ===
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
