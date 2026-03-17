<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>لعبة القفز</title>

  <style>
    body {
      margin: 0;
      background: black;
      text-align: center;
      color: white;
      font-family: Arial;
    }

   #gameArea {
     position: relative;
     width: 600px;
     height: 300px;
     margin: auto;
     overflow: hidden;
     border: 2px solid #267ff5;

     background-image: url('background.png'); /* الخلفية */
     background-size: cover; /* تغطي الشاشة */
     background-position: center;
   }

    #player {
      position: absolute;
      bottom: 0;
      left: 40px;
      width: 100px;
      height: 120px;
      background-image: url('player.png');
      background-size: contain;
      background-repeat: no-repeat;
    }

    .obstacle {
      position: absolute;
      bottom: 0;
      width: 60px;
      height: 120px;
      background-image: url('tree.png');
      background-size: contain;
      background-repeat: no-repeat;
    }

    #score {
      font-size: 20px;
      margin: 10px;
    }
  </style>
</head>

<body>

<h2>🎮 لعبة القفز</h2>
<p id="score">المسافة: 0 م</p>

<div id="gameArea">
  <div id="player"></div>
</div>

<script>
let player = document.getElementById("player");
let gameArea = document.getElementById("gameArea");
let scoreText = document.getElementById("score");

let y = 0;
let jumping = false;
let obstacles = [];
let distance = 0;
let speed = 4;
let gameOver = false;

// القفز
function jump() {
  if (jumping) return;
  jumping = true;

  let up = setInterval(() => {
    if (y >= 100) {
      clearInterval(up);

      let down = setInterval(() => {
        if (y <= 0) {
          y = 0;
          jumping = false;
          clearInterval(down);
        } else {
          y -= 5;
          player.style.bottom = y + "px";
        }
      }, 20);

    } else {
      y += 5;
      player.style.bottom = y + "px";
    }
  }, 20);
}

// الضغط في أي مكان
document.body.addEventListener("click", jump);

// إنشاء الحواجز
function createObstacle() {
  let obs = document.createElement("div");
  obs.classList.add("obstacle");
  obs.style.left = "600px";
  gameArea.appendChild(obs);
  obstacles.push(obs);
}
setInterval(createObstacle, 2000);

// الحركة
function move() {
  obstacles.forEach((obs, i) => {
    let left = parseInt(obs.style.left);
    left -= speed;
    obs.style.left = left + "px";

    if (left < -40) {
      obs.remove();
      obstacles.splice(i, 1);
    }

    // الاصطدام
    if (!gameOver &&
        left < 60 &&
        left > 40 &&
        y < 40) {

      gameOver = true;
      alert("💥 خسرت! المسافة: " + Math.floor(distance));
      location.reload();
    }
  });
}

// النقاط والسرعة
function update() {
  distance += 0.1;
  scoreText.innerHTML = "المسافة: " + Math.floor(distance) + " م";

  if (Math.floor(distance) % 100 === 0) {
    speed += 0.2;
  }
}

setInterval(() => {
  move();
  update();
}, 20);

</script>

</body>
</html>
