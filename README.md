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
  position: fixed;   /* ← VIKTIGT: ändrat här */
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

let x = window.innerWidth * 0.55;
let y = window.innerHeight * 0.65;
const speed = 14;
const dangerRadius = 150;
let hasMoved = false;

// Gör knappen placerbar med left/top
nejButton.style.position = "fixed";
nejButton.style.left = x + "px";
nejButton.style.top = y + "px";

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

    x -= nx * speed;
    y -= ny * speed;

    if (!hasMoved) {
      jaButton.style.transform = "scale(1.6)";
      jaButton.style.transition = "transform 0.2s ease";
      hasMoved = true;
    }
  }

  const screenW = window.innerWidth;
  const screenH = window.innerHeight;

  // === RIKTIG WRAP AROUND ===
  if (x + rect.width < 0) x = screenW;          // ut vänster → in från höger
  if (x > screenW) x = -rect.width;            // ut höger → in från vänster
  if (y + rect.height < 0) y = screenH;         // ut upp → in nerifrån
  if (y > screenH) y = -rect.height;            // ut ner → in uppifrån

  nejButton.style.left = x + "px";
  nejButton.style.top = y + "px";
});
</script>


</body>
</html>
