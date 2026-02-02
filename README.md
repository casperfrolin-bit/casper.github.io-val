<html>
<head>
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
  background-color: #FFFFFF; /* vit */
  border-radius: 20px;
  display: flex;
  justify-content: center; /* horisontell centrering */
  align-items: center;     /* vertikal centrering av texten */
  text-align: center;
  padding: 10px;
  font-family: Arial, sans-serif;
  font-size: 16px;
  color: #333;
  word-wrap: break-word;   /* bryter texten om den är för lång */
}
</style>
</head>

<body>

<div class="center-box">
  <img src="https://thumbs.dreamstime.com/b/print-206284399.jpg" alt="Bild">
  <div class="text-box">
    Här är texten du kan ändra i koden. Om den blir längre växer rutan automatiskt!
  </div>
</div>

</body>
</html>
