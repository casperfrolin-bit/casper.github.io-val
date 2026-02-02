<html>
<head>
<!-- Ladda Noto Sans Japanese från Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@700&display=swap" rel="stylesheet">

<style>
body {
  margin: 0;
  height: 100vh;
  background-color: #ffd1dc;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
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

/* Gemensam knappstil */
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
  transition: transform 0.1s;
}

/* Ja-knapp */
.button-ja {
  width: 150px;
  height: 60px;
  font-size: 18px;
  background-color: #ff69b4;
  color: white;
  margin-top: 20px;
}

/* Nej-knapp */
.button-nej {
  width: 150px;
  height: 60px;
  font-size: 18px;
  background-color: #b0b0b0;
  color: #333;
  position: absolute; /* fri rörelse */
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

// Startposition: mitten under Ja-knappen
let posX = window.innerWidth / 2 - nejButton.offsetWidth / 2 + 80; // lite åt höger
let posY = window.innerHeight / 2 + 50;
nejButton.style.left = posX + 'px';
nejButton.style.top = posY + 'px';

// Lyssna på musrörelser
document.addEventListener('mousemove', e => {
  const mouseX = e.clientX;
  const mouseY = e.clientY;

  const rect = nejButton.getBoundingClientRect();
  const buttonCenterX = rect.left + rect.width / 2;
  const buttonCenterY = rect.top + rect.height / 2;

  const distance = Math.hypot(mouseX - buttonCenterX, mouseY - buttonCenterY);

  if(distance < 150) { // När musen är nära
    // Flytta knappen bort beroende på musens position
    const offsetX = (buttonCenterX - mouseX) / distance * (100 + Math.random() * 50);
    const offsetY = (buttonCenterY - mouseY) / distance * (50 + Math.random() * 50);

    posX += offsetX;
    posY += offsetY;

    // Håll inom fönstret
    posX = Math.max(0, Math.min(posX, window.innerWidth - nejButton.offsetWidth));
    posY = Math.max(0, Math.min(posY, window.innerHeight - nejButton.offsetHeight));

    // Ändra knappens storlek lite när den flyttar sig
    const scale = Math.max(0.7, 1 - (150 - distance)/400);
    nejButton.style.transform = `scale(${scale})`;

    nejButton.style.left = posX + 'px';
    nejButton.style.top = posY + 'px';
  }
});
</script>

</body>
</html>
