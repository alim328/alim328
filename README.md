 <img width="1536" height="1024" alt="ChatGPT Image 23 May 2026 09_48_49" src="https://github.com/user-attachments/assets/426218e4-0e35-4311-8be1-680ed0fde024" />
 <h1 align="center">Merhaba, Ben AlİM! 👋</h1>


<h3 align="center">Oyun, Web ve Robotik Tutkunu Bir Software Developer</h3>

<p align="center">
  <img src="https://komarev.com/ghpvc/?username=alim328&label=Profil+Görüntülenme&color=blue&style=flat" alt="alim328" />
</p>

<br>

### 🚀 Hakkımda

* 💻 **Yazılım:** Web geliştirme tarafında projeler üretiyor; Python ve JavaScript üzerinde çalışıyorum.
* 🤖 **Donanım:** Sensörler ve LCD ekranlar kullanarak çeşitli devre projeleri tasarlıyorum.
* 🎮 **Oyun Dünyası:** Yeni oyun mekaniklerini incelemeyi, stratejiler kurmayı ve oyun projeleri geliştirmeyi seviyorum.

---

### 🛠️ Kullandığım Teknolojiler ve Diller

<p align="left">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" />
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" />
  <img src="https://img.shields.io/badge/Arduino-00979D?style=for-the-badge&logo=arduino&logoColor=white" />
</p>

---

### 📊 GitHub İstatistikleri

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=alim328&show_icons=true&theme=tokyonight&hide_border=true" alt="GitHub Stats" />
</p>


<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>alim328 - Fantastiko Snake</title>
    <style>
        body {
            background-color: #121212;
            color: #ffffff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }
        h1 {
            color: #00ffcc;
            margin-bottom: 5px;
            text-shadow: 0 0 10px #00ffcc;
        }
        #score-board {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 15px;
            color: #ff0055;
            text-shadow: 0 0 5px #ff0055;
        }
        canvas {
            border: 4px solid #00ffcc;
            background-color: #050505;
            box-shadow: 0 0 25px #00ffcc;
            border-radius: 8px;
        }
        .controls {
            margin-top: 15px;
            color: #888;
            font-size: 14px;
            text-align: center;
        }
        .highlight {
            color: #00ffcc;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <h1>🐍 FANTASTİKO SNAKE 🐍</h1>
    <div id="score-board">SKOR: <span id="score">0</span></div>
    
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    
    <div class="controls">
        Hareket etmek için <span class="highlight">YÖN TUŞLARINI</span> veya <span class="highlight">W, A, S, D</span> tuşlarını kullan!<br>
        Kendine çarparsan veya duvara toslarsan oyun biter.
    </div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const scoreElement = document.getElementById("score");

        const gridSize = 20;
        const tileCount = canvas.width / gridSize;

        // Yılanın başlangıç konumu ve hızı
        let snake = [{x: 10, y: 10}];
        let food = {x: 5, y: 5};
        let dx = 1; // Yatay hız (Sağa doğru başlar)
        let dy = 0; // Dikey hız
        let score = 0;
        let gameSpeed = 90; // Oyun hızı (Milisaniye cinsinden, düştükçe hızlanır)
        let gameInterval;

        // Oyunu başlatan ana döngü
        function gameLoop() {
            if (hasGameEnded()) {
                alert("OYUN BİTTİ! 💥\nToplam Skorun: " + score + "\nYeniden başlamak için Tamam'a bas!");
                resetGame();
                return;
            }

            clearCanvas();
            drawFood();
            moveSnake();
            drawSnake();
        }

        // Arka planı temizleme
        function clearCanvas() {
            ctx.fillStyle = "#050505";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
        }

        // Yılanı ekrana çizme
        function drawSnake() {
            snake.forEach((part, index) => {
                // Yılanın kafası neon yeşil, gövdesi koyu yeşil olur
                ctx.fillStyle = (index === 0) ? "#00ffcc" : "#00b386";
                ctx.strokeStyle = "#050505";
                
                ctx.fillRect(part.x * gridSize, part.y * gridSize, gridSize, gridSize);
                ctx.strokeRect(part.x * gridSize, part.y * gridSize, gridSize, gridSize);
            });
        }

        // Yılanı hareket ettirme ve yem yeme kontrolü
        function moveSnake() {
            const head = {x: snake[0].x + dx, y: snake[0].y + dy};
            snake.unshift(head);

            // Eğer yem yendiyse
            if (snake[0].x === food.x && snake[0].y === food.y) {
                score += 10;
                scoreElement.innerText = score;
                generateFood();
            } else {
                // Yem yenmediyse kuyruğu kısalt ki yılan ilerlemiş olsun
                snake.pop();
            }
        }

        // Rastgele yem oluşturma
        function generateFood() {
            food.x = Math.floor(Math.random() * tileCount);
            food.y = Math.floor(Math.random() * tileCount);
            
            // Yemin yılanın üstünde çıkmasını engelle
            snake.forEach(part => {
                if (part.x === food.x && part.y === food.y) generateFood();
            });
        }

        // Yemi ekrana çizme (Neon Pembe/Kırmızı)
        function drawFood() {
            ctx.fillStyle = "#ff0055";
            ctx.shadowBlur = 10;
            ctx.shadowColor = "#ff0055";
            ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize, gridSize);
            ctx.shadowBlur = 0; // Diğer çizimlerin parlamasını sıfırla
        }

        // Tuş kontrolleri (Geriye doğru aniden dönmeyi engeller)
        function changeDirection(event) {
            const keyPressed = event.keyCode;
            const LEFT = 37, UP = 38, RIGHT = 39, DOWN = 40;
            const W = 87, A = 65, S = 83, D = 68;

            const goingUp = dy === -1;
            const goingDown = dy === 1;
            const goingRight = dx === 1;
            const goingLeft = dx === -1;

            if ((keyPressed === LEFT || keyPressed === A) && !goingRight) { dx = -1; dy = 0; }
            if ((keyPressed === UP || keyPressed === W) && !goingDown) { dx = 0; dy = -1; }
            if ((keyPressed === RIGHT || keyPressed === D) && !goingLeft) { dx = 1; dy = 0; }
            if ((keyPressed === DOWN || keyPressed === S) && !goingUp) { dx = 0; dy = 1; }
        }

        // Çarpışma kontrolleri
        function hasGameEnded() {
            // Kendine çarpma kontrolü
            for (let i = 4; i < snake.length; i++) {
                if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) return true;
            }
            // Duvarlara çarpma kontrolü
            const hitLeftWall = snake[0].x < 0;
            const hitRightWall = snake[0].x >= tileCount;
            const hitTopWall = snake[0].y < 0;
            const hitBottomWall = snake[0].y >= tileCount;

            return hitLeftWall || hitRightWall || hitTopWall || hitBottomWall;
        }

        // Oyunu sıfırlama
        function resetGame() {
            snake = [{x: 10, y: 10}];
            food = {x: 5, y: 5};
            dx = 1;
            dy = 0;
            score = 0;
            scoreElement.innerText = score;
        }

        // Dinleyicileri ve zamanlayıcıyı başlat
        document.addEventListener("keydown", changeDirection);
        gameInterval = setInterval(gameLoop, gameSpeed);
    </script>
</body>
</html>
