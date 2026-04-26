# araba-oyunu-bedava
yeni yaptım lutfen oynayın
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<title>Araba Oyunu</title>
<style>
body {
  margin: 0;
  background: #111;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

canvas {
  background: #2d2d2d;
  border: 3px solid white;
}
</style>
</head>
<body>

<canvas id="game" width="400" height="600"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let car, obstacles, speed, score, gameOver;

function reset() {
  car = { x: 180, y: 500, w: 40, h: 70 };
  obstacles = [];
  speed = 4;
  score = 0;
  gameOver = false;
}

reset();

document.addEventListener("keydown", (e) => {
  if (gameOver) return;

  if (e.key === "ArrowLeft" && car.x > 0) car.x -= 25;
  if (e.key === "ArrowRight" && car.x < 360) car.x += 25;
});

function spawn() {
  if (gameOver) return;

  obstacles.push({
    x: Math.random() * 360,
    y: -80,
    w: 40,
    h: 70
  });
}

setInterval(spawn, 1000);

function drawCar() {
  ctx.fillStyle = "red";
  ctx.fillRect(car.x, car.y, car.w, car.h);
}

function drawObstacles() {
  obstacles.forEach((o, i) => {
    o.y += speed;

    ctx.fillStyle = "yellow";
    ctx.fillRect(o.x, o.y, o.w, o.h);

    // ÇARPIŞMA
    if (
      car.x < o.x + o.w &&
      car.x + car.w > o.x &&
      car.y < o.y + o.h &&
      car.y + car.h > o.y
    ) {
      gameOver = true;
      setTimeout(reset, 1000);
    }

    if (o.y > 650) {
      obstacles.splice(i, 1);
      score++;
      if (score % 5 === 0) speed++;
    }
  });
}

function drawScore() {
  ctx.fillStyle = "white";
  ctx.font = "20px Arial";
  ctx.fillText("Skor: " + score, 10, 30);
}

function loop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  drawCar();
  drawObstacles();
  drawScore();

  if (gameOver) {
    ctx.fillStyle = "red";
    ctx.font = "40px Arial";
    ctx.fillText("ÇARPTIN!", 110, 300);
  }

  requestAnimationFrame(loop);
}

loop();
</script>

</body>
</html>
