<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pomodoro Escape</title>
    <style>
        body {
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
        }
        canvas {
            background: white;
            border: 2px solid black;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        canvas.width = 800;
        canvas.height = 400;

        // Pomodoro settings
        const pomodoro = {
            x: 50,
            y: 300,
            width: 40,
            height: 40,
            color: "red",
            velocityY: 0,
            isJumping: false
        };

        // Gravity
        const gravity = 0.5;
        const jumpPower = -10;

        // Obstacles
        let obstacles = [
            { x: 300, y: 320, width: 40, height: 40 },
            { x: 600, y: 280, width: 50, height: 80 }
        ];

        // Input listener
        document.addEventListener("keydown", (event) => {
            if (event.code === "Space" && !pomodoro.isJumping) {
                pomodoro.velocityY = jumpPower;
                pomodoro.isJumping = true;
            }
        });

        // Game loop
        function update() {
            pomodoro.y += pomodoro.velocityY;
            pomodoro.velocityY += gravity;
            
            if (pomodoro.y >= 300) {
                pomodoro.y = 300;
                pomodoro.isJumping = false;
            }
            
            obstacles.forEach(obstacle => obstacle.x -= 3);
            
            if (obstacles.length && obstacles[0].x < -50) {
                obstacles.shift();
                obstacles.push({ x: 800, y: 320, width: 40, height: 40 });
            }
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = pomodoro.color;
            ctx.fillRect(pomodoro.x, pomodoro.y, pomodoro.width, pomodoro.height);
            
            ctx.fillStyle = "black";
            obstacles.forEach(obstacle => {
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
            });
        }

        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
    </script>
</body>
</html>
