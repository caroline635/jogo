<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <title>Evite o Quadrado Vermelho</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    canvas { background: #f0f0f0; display: block; margin: auto; margin-top: 50px; }
    body { font-family: Arial, sans-serif; text-align: center; }
  </style>
</head>
<body>
  <h1>Evite o Quadrado Vermelho</h1>
  <canvas id="gameCanvas" width="400" height="600"></canvas>
  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    const player = {
      x: 180,
      y: 550,
      width: 40,
      height: 40,
      color: "blue",
      speed: 5
    };

    let enemies = [];
    let score = 0;
    let gameOver = false;

    function drawPlayer() {
      ctx.fillStyle = player.color;
      ctx.fillRect(player.x, player.y, player.width, player.height);
    }

    function drawEnemies() {
      ctx.fillStyle = "red";
      enemies.forEach(enemy => {
        ctx.fillRect(enemy.x, enemy.y, enemy.size, enemy.size);
      });
    }

    function updateEnemies() {
      enemies.forEach(enemy => {
        enemy.y += enemy.speed;
      });

      // Remover inimigos fora da tela
      enemies = enemies.filter(enemy => enemy.y < canvas.height);

      // Adicionar novos inimigos
      if (Math.random() < 0.03) {
        const size = 30;
        enemies.push({
          x: Math.random() * (canvas.width - size),
          y: -size,
          size: size,
          speed: 2 + Math.random() * 3
        });
      }
    }

    function checkCollision() {
      enemies.forEach(enemy => {
        if (
          player.x < enemy.x + enemy.size &&
          player.x + player.width > enemy.x &&
          player.y < enemy.y + enemy.size &&
          player.y + player.height > enemy.y
        ) {
          gameOver = true;
        }
      });
    }

    function drawScore() {
      ctx.fillStyle = "black";
      ctx.font = "20px Arial";
      ctx.fillText("Score: " + score, 10, 30);
    }

    function updateGame() {
      if (gameOver) {
        ctx.fillStyle = "black";
        ctx.font = "40px Arial";
        ctx.fillText("Game Over", 100, 300);
        return;
      }

      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawPlayer();
      drawEnemies();
      updateEnemies();
