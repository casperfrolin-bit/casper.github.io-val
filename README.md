<html>
<head>
<!-- Ladda Noto Sans Japanese från Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@700&display=swap" rel="stylesheet">

<style>
body {
  margin: 0;
  height: 100vh;
  background-color: #ffd1dc; /* pastel pink */
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

/* Gemensam knappstil */
.button {
  width: 200px;
  height: 70px;
  font-family: 'Noto Sans JP', sans-serif;
  font-weight: 700;
  font-size: 20px;
  border: none;
  outline: none;
  border-radius: 50px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  transition: transform 0.3s ease-out;
}

.button:hover {
  transform: scale(1.05);
}

/* Färger för knapparna */
.button-ja {
  background-color: #ff69b4;
  color: white;
}

.button-nej {
  background-color: #dcdcdc;
  color: #333;
  position: relative; /* VIKTIGT för rörelsen */
}

/* Container för knappar */
.button-container {
  display: flex;
  gap: 40px;
  margin-top: 20px;
  align-self: center;
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
    <button class="button button-nej">Nej</button>
  </div>
</div>

<script>
const nejButton = document.querySelector('.button-nej');

let offsetX = 0;
let offsetY = 0;
const maxDistance = 120;

document.addEventListener('mousemove', (e) => {
  const rect = nejButton.getBoundingClientRect();

  const buttonCenterX = rect.left + rect.width / 2;
  const buttonCenterY = rect.top + rect.height / 2;

  const dx = e.clientX - buttonCenterX;
  const dy = e.clientY - buttonCenterY;

  const distance = Math.sqrt(dx * dx + dy * dy);

  if (distance < maxDistance) {
    const force = (maxDistance - distance) / maxDistance;

    offsetX -= dx * force * 0.05;
    offsetY -= dy * force * 0.05;

    nejButton.style.transform =
      `translate(${offsetX}px, ${offsetY}px)`;
  }
});
</script>

</body>
</html>
