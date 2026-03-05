<!DOCTYPE html>
<html>
<head>
    <title>Trump Run Game</title>
    <style>
        canvas {
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="800" height="600"></canvas>
    <script>
        var canvas = document.getElementById('gameCanvas');
        var ctx = canvas.getContext('2d');

        var player = {
            x: 50,
            y: canvas.height / 2,
            width: 50,
            height: 50,
            speed: 5,
        };

        var enemies = []; 
        var money = 0;

        function createEnemy() {
            var enemy = {
                x: canvas.width,
                y: Math.random() * canvas.height,
                width: 50,
                height: 50,
                speed: 2 + Math.random() * 3
            };
            enemies.push(enemy);
        }

        function drawPlayer() {
            ctx.fillStyle = 'blue';
            ctx.fillRect(player.x, player.y, player.width, player.height);
        }

        function drawEnemies() {
            ctx.fillStyle = 'red';
            enemies.forEach(function(enemy) {
                ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);
                enemy.x -= enemy.speed;

                // Check for collision
                if (player.x < enemy.x + enemy.width &&
                    player.x + player.width > enemy.x &&
                    player.y < enemy.y + enemy.height &&
                    player.height + player.y > enemy.y) {
                    alert('Game Over!');
                    document.location.reload();
                }
            });
        }

        function collectMoney() {
            money += 1;
            console.log('Money: ' + money);
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawPlayer();
            drawEnemies();

            if (Math.random() < 0.02) {
                createEnemy();
            }

            requestAnimationFrame(gameLoop);
        }

        window.addEventListener('keydown', function(event) {
            switch(event.key) {
                case 'ArrowUp':
                    player.y -= player.speed;
                    break;
                case 'ArrowDown':
                    player.y += player.speed;
                    break;
                case ' ': // Space to collect money
                    collectMoney();
                    break;
            }
        });

        gameLoop();
    </script>
</body>
</html>