<html>
<head>
<!-- Ladda Noto Sans Japanese från Google Fonts -->
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

.center-box img {
  width: 200px;
  height: 200px;
  border-radius: 10px;
  margin-bottom: 20px;
}

.text-box {
  width: 700px;
  background-color: #FFFFFF;
  border-radius: 20px;
  padding: 10px;
  font-family: 'Noto Sans JP', sans-serif;
  font-weight: 700;
  font-size: 40px;
  color: #333;
  text-align: center;
  word-wrap: break-word;
}

.button {
  font-family: 'Noto Sans JP', sans-serif;
  font-weight: 700;
  border: none;
  outline: none;
  border-radius: 50px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  transition: transform 0.05s;
}

/* Ja-knapp */
.button-ja {
  width: 150px;
  height: 60px;
  font-size: 18px;
  background-color: #ff69b4;
  color: white;
  position: absolute;
  top: 400px; /* justera höjd under texten */
  left: calc(50% - 120px); /* mer åt vänster */
}

/* Nej-knapp */
.button-nej {
  width: 150px;
  height: 60px;
  font-size: 18px;
  background-color: #b0b0b0;
  color: #333;
  position: absolute; /* fri rörelse */
  top: 400px; /* samma höjd som Ja-knappen */
  left: calc(50% + 120px); /* mer åt höger */
}
</style>
</head>

<body>

<div class="center-box">
  <img src="https://thumbs.dreamstime.com/b/print-206284399.jpg" alt="Bild">
  <div class="text-box">
    ... vill du bli min valentine?
  </div>
  <div class="button-container">
    <button class="button button-ja">Ja</button>
    <button class="button button-nej" id="nejButton">Nej</button>
  </div>
</div>

<script>
const nejButton = document.getElementById('nejButton');

// Startposition är där vi satte den i CSS (mer åt höger)
let posX = nejButton.offsetLeft;
let posY = nejButton.offsetTop;

nejButton.style.left = posX + 'px';
nejButton.style.top = posY + 'px';

document.addEventListener('mousemove', e => {
  const mouseX = e.clientX;
  const mouseY = e.clientY;

  const rect = nejButton.getBoundingClientRect();
  const buttonCenterX = rect.left + rect.width / 2;
  const buttonCenterY = rect.top + rect.height / 2;

  const distance = Math.hypot(mouseX - buttonCenterX, mouseY - buttonCenterY);

  if(distance < 150) { 
    // Flytta knappen fritt
    const dx = (buttonCenterX - mouseX) / distance * (50 + Math.random() * 100);
    const dy = (buttonCenterY - mouseY) / distance * (30 + Math.random() * 70);

    posX += dx;
    posY += dy;

    // Wrap-around: kommer in från motsatt sida
    if(posX + nejButton.offsetWidth < 0) posX = window.innerWidth;
    if(posX > window.innerWidth) posX = -nejButton.offsetWidth;
    if(posY + nejButton.offsetHeight < 0) posY = window.innerHeight;
    if(posY > window.innerHeight) posY = -nejButton.offsetHeight;

    // Skala ner knappen när den flyttar sig
    const scale = Math.max(0.5, 1 - (150 - distance)/300);
    nejButton.style.transform = `scale(${scale})`;

    nejButton.style.left = posX + 'px';
    nejButton.style.top = posY + 'px';
  } else {
    // Återställ normal storlek
    nejButton.style.transform = 'scale(1)';
  }
});
</script>

</body>
</html>
