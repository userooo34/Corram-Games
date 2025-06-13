# Corram-Games<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Carrom Game</title>
  <style>
    body {
      margin: 0;
      background: #1a1a1a;
    }
    canvas {
      display: block;
      margin: 20px auto;
      background: #f9e4b7;
      border: 10px solid #8b4513;
      border-radius: 20px;
    }
  </style>
</head>
<body>
  <canvas id="carrom" width="600" height="600"></canvas>

  <script>
    const canvas = document.getElementById("carrom");
    const ctx = canvas.getContext("2d");

    let striker = {
      x: 300,
      y: 550,
      radius: 15,
      color: "#000",
      dx: 0,
      dy: 0,
      isMoving: false,
    };

    function drawBoard() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      let pockets = [
        [0, 0], [600, 0], [0, 600], [600, 600]
      ];
      pockets.forEach(([x, y]) => {
        ctx.beginPath();
        ctx.arc(x, y, 20, 0, Math.PI * 2);
        ctx.fillStyle = "#111";
        ctx.fill();
      });

      ctx.beginPath();
      ctx.arc(striker.x, striker.y, striker.radius, 0, Math.PI * 2);
      ctx.fillStyle = striker.color;
      ctx.fill();
    }

    function update() {
      if (striker.isMoving) {
        striker.x += striker.dx;
        striker.y += striker.dy;
        striker.dx *= 0.98;
        striker.dy *= 0.98;
        if (Math.abs(striker.dx) < 0.1 && Math.abs(striker.dy) < 0.1) {
          striker.isMoving = false;
        }
      }
    }

    function gameLoop() {
      update();
      drawBoard();
      requestAnimationFrame(gameLoop);
    }

    canvas.addEventListener("click", (e) => {
      if (!striker.isMoving) {
        let rect = canvas.getBoundingClientRect();
        let mx = e.clientX - rect.left;
        let my = e.clientY - rect.top;

        let angle = Math.atan2(my - striker.y, mx - striker.x);
        let speed = 8;

        striker.dx = Math.cos(angle) * speed;
        striker.dy = Math.sin(angle) * speed;
        striker.isMoving = true;
      }
    });

    drawBoard();
    gameLoop();
  </script>
</body>
</html>
