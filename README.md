<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
            background-color: #f4f4f4;
        }

        canvas {
            border: 1px solid #333;
        }
    </style>
</head>
<body>

    <h1>Snake Game</h1>
    <p>Use arrow keys to control the snake.</p>

    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const boxSize = 20;
        let snake = [{ x: 10, y: 10 }];
        let direction = "right";
        let food = { x: 15, y: 15 };

        function draw() {
            // Clear the canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw the snake
            ctx.fillStyle = "#4CAF50";
            for (let i = 0; i < snake.length; i++) {
                ctx.fillRect(snake[i].x * boxSize, snake[i].y * boxSize, boxSize, boxSize);
            }

            // Draw the food
            ctx.fillStyle = "#FF0000";
            ctx.fillRect(food.x * boxSize, food.y * boxSize, boxSize, boxSize);

            // Move the snake
            let headX = snake[0].x;
            let headY = snake[0].y;

            if (direction === "right") headX++;
            if (direction === "left") headX--;
            if (direction === "up") headY--;
            if (direction === "down") headY++;

            // Check for collisions with the walls
            if (headX < 0 || headY < 0 || headX >= canvas.width / boxSize || headY >= canvas.height / boxSize) {
                alert("Game over! Refresh the page to play again.");
                return;
            }

            // Check for collisions with the food
            if (headX === food.x && headY === food.y) {
                // Increase the length of the snake
                snake.unshift({ x: headX, y: headY });

                // Generate new food coordinates
                food = {
                    x: Math.floor(Math.random() * (canvas.width / boxSize)),
                    y: Math.floor(Math.random() * (canvas.height / boxSize))
                };
            } else {
                // Remove the tail of the snake
                snake.pop();

                // Add a new head to the snake
                snake.unshift({ x: headX, y: headY });
            }
        }

        function changeDirection(event) {
            switch (event.key) {
                case "ArrowUp":
                    direction = "up";
                    break;
                case "ArrowDown":
                    direction = "down";
                    break;
                case "ArrowLeft":
                    direction = "left";
                    break;
                case "ArrowRight":
                    direction = "right";
                    break;
            }
        }

        // Listen for arrow key presses
        document.addEventListener("keydown", changeDirection);

        // Run the draw function every 100 milliseconds
        setInterval(draw, 300);
    </script>

</body>
</html>
