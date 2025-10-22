<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Snake Game</title>
  <style>
    body {
      margin: 0;
      background: #0a0a0a;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    canvas {
      background: #111;
      border: 3px solid #00ff99;
      border-radius: 10px;
      image-rendering: pixelated;
    }
  </style>
</head>
<body>
  <canvas id="game" width="400" height="400"></canvas>
  <script>
    const canvas = document.getElementById('game');
    const ctx = canvas.getContext('2d');

    const box = 20;
    let snake = [{ x: 9 * box, y: 10 * box }];
    let direction = 'RIGHT';
    let food = {
      x: Math.floor(Math.random() * 19 + 1) * box,
      y: Math.floor(Math.random() * 19 + 1) * box
    };
    let score = 0;

    document.addEventListener('keydown', e => {
      if (e.key === 'ArrowUp' && direction !== 'DOWN') direction = 'UP';
      else if (e.key === 'ArrowDown' && direction !== 'UP') direction = 'DOWN';
      else if (e.key === 'ArrowLeft' && direction !== 'RIGHT') direction = 'LEFT';
      else if (e.key === 'ArrowRight' && direction !== 'LEFT') direction = 'RIGHT';
    });

    function draw() {
      ctx.clearRect(0, 0, 400, 400);

      // desenha comida
      ctx.fillStyle = '#ff3366';
      ctx.fillRect(food.x, food.y, box, box);

      // desenha cobra
      for (let i = 0; i < snake.length; i++) {
        ctx.fillStyle = i === 0 ? '#00ff99' : '#00cc77';
        ctx.fillRect(snake[i].x, snake[i].y, box - 1, box - 1);
      }

      // posição inicial
      let snakeX = snake[0].x;
      let snakeY = snake[0].y;

      if (direction === 'LEFT') snakeX -= box;
      if (direction === 'UP') snakeY -= box;
      if (direction === 'RIGHT') snakeX += box;
      if (direction === 'DOWN') snakeY += box;

      // se comer a comida
      if (snakeX === food.x && snakeY === food.y) {
        score++;
        food = {
          x: Math.floor(Math.random() * 19 + 1) * box,
          y: Math.floor(Math.random() * 19 + 1) * box
        };
      } else {
        snake.pop();
      }

      let newHead = { x: snakeX, y: snakeY };

      // colisão
      if (
        snakeX < 0 ||
        snakeY < 0 ||
        snakeX >= canvas.width ||
        snakeY >= canvas.height ||
        snake.some(seg => seg.x === newHead.x && seg.y === newHead.y)
      ) {
        clearInterval(game);
        alert(`Game Over! Pontuação: ${score}`);
      }

      snake.unshift(newHead);
    }

    const game = setInterval(draw, 100);
  </script>
</body>
</html>

