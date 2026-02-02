<html>
<head>
<!-- Ladda Oswald-font från Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@700&display=swap" rel="stylesheet">

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
  padding-top: 20px; /* avstånd från toppen */
}

.center-box img {
  width: 200px;         
  height: 200px;        
  border-radius: 10px; 
  margin-bottom: 20px; /* avstånd mellan bild och textruta */
}

.text-box {
  width: 200px;
  background-color: #FFFFFF; /* ljusare rosa */
  border-radius: 20px;
  padding: 10px;

  /* Bold Oswald-font */
  font-family: 'Oswald', sans-serif;
  font-weight: 700;
  font-size: 40px;
  color: #333;

  text-align: center;
  word-wrap: break-word; /* bryter texten om den blir för lång */
}
</style>
</head>

<body>

<div class="center-box">
  <img src="https://thumbs.dreamstime.com/b/print-206284399.jpg" alt="Bild">
  <div class="text-box">
    Här är texten du kan ändra i koden
  </div>
</div>

</body>
</html>
