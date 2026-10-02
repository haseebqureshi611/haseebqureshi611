Hi, I am Haseeb Qureshi 👋

🎓 Software Engineering Student from Azad Kashmir, Pakistan

I’m passionate about Web Development, Software Engineering, and Digital Marketing. I enjoy learning new technologies and building practical projects.

👨‍💻 About Me
	•	🌱 Currently learning Web Development & Programming
	•	💻 Interested in Software & Web Development
	•	🤝 Looking to collaborate on Web & Software Projects
	•	🚀 Always learning and exploring new technologies
	•	⚡ Fun fact: I love turning ideas into real projects!

🛠️ Skills

HTML CSS JavaScript C++ WordPress Shopify SEO Digital Marketing

📫 Contact Me

📧 Email: ahaseeb.qureshi118@gmail.com

Let’s connect, collaborate & build something amazing!
## 🌐 Socials:
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Snake Game</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    min-height: 100vh;
    background: #0d1117;
    color: white;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
}

.game {
    width: 95%;
    max-width: 520px;
    text-align: center;
}

h1 {
    margin-bottom: 5px;
    font-size: 32px;
}

.subtitle {
    color: #8b949e;
    margin-bottom: 20px;
}

.info {
    display: flex;
    justify-content: space-between;
    background: #161b22;
    border: 1px solid #30363d;
    padding: 14px 20px;
    border-radius: 10px;
    margin-bottom: 15px;
}

canvas {
    width: 100%;
    max-width: 500px;
    aspect-ratio: 1;
    background: #010409;
    border: 2px solid #30363d;
    border-radius: 12px;
}

button {
    margin-top: 15px;
    padding: 11px 22px;
    border: 0;
    border-radius: 8px;
    background: #238636;
    color: white;
    cursor: pointer;
    font-size: 16px;
}

.controls {
    margin: 18px auto;
    display: grid;
    grid-template-columns: repeat(3, 60px);
    gap: 7px;
    justify-content: center;
}

.controls button {
    margin: 0;
    padding: 15px 5px;
    background: #21262d;
}

.blank {
    visibility: hidden;
}

.message {
    color: #8b949e;
    font-size: 14px;
}
</style>
</head>

<body>

<div class="game">

<h1>🐍 Snake Game</h1>

<p class="subtitle">Created by Haseeb Qureshi</p>

<div class="info">
<span>Score: <b id="score">0</b></span>
<span>High Score: <b id="high">0</b></span>
</div>

<canvas id="canvas" width="500" height="500"></canvas>

<button onclick="restart()">🔄 Restart</button>

<div class="controls">

<button class="blank"></button>
<button onclick="move('up')">⬆️</button>
<button class="blank"></button>

<button onclick="move('left')">⬅️</button>
<button onclick="move('down')">⬇️</button>
<button onclick="move('right')">➡️</button>

</div>

<p class="message">Use Arrow Keys / WASD to play</p>

</div>

<script>

const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

const size = 25;
const tile = canvas.width / size;

let snake;
let food;
let direction;
let nextDirection;
let score = 0;
let highScore = Number(localStorage.getItem("snakeHighScore")) || 0;
let running = false;
let timer;

document.getElementById("high").textContent = highScore;

function start() {

    snake = [
        {x: 12, y: 12},
        {x: 11, y: 12},
        {x: 10, y: 12}
    ];

    direction = {x: 1, y: 0};
    nextDirection = {x: 1, y: 0};

    score = 0;
    running = true;

    document.getElementById("score").textContent = score;

    createFood();

    clearInterval(timer);

    timer = setInterval(game, 100);

    draw();
}

function game() {

    if (!running) return;

    direction = nextDirection;

    const head = {
        x: snake[0].x + direction.x,
        y: snake[0].y + direction.y
    };

    if (
        head.x < 0 ||
        head.x >= size ||
        head.y < 0 ||
        head.y >= size
    ) {
        endGame();
        return;
    }

    if (
        snake.some(part =>
            part.x === head.x &&
            part.y === head.y
        )
    ) {
        endGame();
        return;
    }

    snake.unshift(head);

    if (
        head.x === food.x &&
        head.y === food.y
    ) {

        score++;

        document.getElementById("score").textContent = score;

        if (score > highScore) {

            highScore = score;

            localStorage.setItem(
                "snakeHighScore",
                highScore
            );

            document.getElementById("high").textContent = highScore;
        }

        createFood();

    } else {

        snake.pop();

    }

    draw();
}

function draw() {

    ctx.fillStyle = "#010409";

    ctx.fillRect(
        0,
        0,
        canvas.width,
        canvas.height
    );

    ctx.strokeStyle = "#161b22";

    for (let i = 0; i <= size; i++) {

        ctx.beginPath();
        ctx.moveTo(i * tile, 0);
        ctx.lineTo(i * tile, canvas.height);
        ctx.stroke();

        ctx.beginPath();
        ctx.moveTo(0, i * tile);
        ctx.lineTo(canvas.width, i * tile);
        ctx.stroke();
    }

    ctx.fillStyle = "#ff4757";

    ctx.beginPath();

    ctx.arc(
        food.x * tile + tile / 2,
        food.y * tile + tile / 2,
        tile / 3,
        0,
        Math.PI * 2
    );

    ctx.fill();

    snake.forEach((part, index) => {

        ctx.fillStyle =
            index === 0 ? "#58ff7a" : "#2ea043";

        ctx.beginPath();

        ctx.roundRect(
            part.x * tile + 1,
            part.y * tile + 1,
            tile - 2,
            tile - 2,
            5
        );

        ctx.fill();

    });
}

function createFood() {

    do {

        food = {
            x: Math.floor(Math.random() * size),
            y: Math.floor(Math.random() * size)
        };

    } while (
        snake.some(
            part =>
                part.x === food.x &&
                part.y === food.y
        )
    );
}

function move(dir) {

    if (!running) return;

    if (dir === "up" && direction.y !== 1) {
        nextDirection = {x: 0, y: -1};
    }

    if (dir === "down" && direction.y !== -1) {
        nextDirection = {x: 0, y: 1};
    }

    if (dir === "left" && direction.x !== 1) {
        nextDirection = {x: -1, y: 0};
    }

    if (dir === "right" && direction.x !== -1) {
        nextDirection = {x: 1, y: 0};
    }
}

function endGame() {

    running = false;

    clearInterval(timer);

    setTimeout(() => {

        alert(
            "🐍 Game Over!\n\nYour Score: " + score
        );

    }, 100);
}

function restart() {
    start();
}

document.addEventListener("keydown", e => {

    const key = e.key.toLowerCase();

    if (key === "arrowup" || key === "w")
        move("up");

    if (key === "arrowdown" || key === "s")
        move("down");

    if (key === "arrowleft" || key === "a")
        move("left");

    if (key === "arrowright" || key === "d")
        move("right");

});

start();

</script>

</body>
</html>

[![Facebook](https://img.shields.io/badge/Facebook-%231877F2.svg?logo=Facebook&logoColor=white)](https://facebook.com/https://www.facebook.com/share/1CopEyTPNK/?mibextid=wwXIfr) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://instagram.com/https://www.instagram.com/haseeb.qurexhi33?stkn=MWgzeDkyMDR3cWhneA%3D%3D&utm_source=qr) [![TikTok](https://img.shields.io/badge/TikTok-%23000000.svg?logo=TikTok&logoColor=white)](https://tiktok.com/@www.tiktok.com/@haseeb.qureshi91) [![X](https://img.shields.io/badge/X-black.svg?logo=X&logoColor=white)](https://x.com/https://x.com/haseebqureztxc?s=11) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://youtube.com/@https://youtu.be/fdDSZGBSeiw?si=WvpZOdW8f09Ed76w) [![email](https://img.shields.io/badge/Email-D14836?logo=gmail&logoColor=white)](mailto:ahaseeb.qureshi118@gmail.com) 

# 💻 Tech Stack:
![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white) ![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![C](https://img.shields.io/badge/c-%2300599C.svg?style=for-the-badge&logo=c&logoColor=white) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![WordPress](https://img.shields.io/badge/WordPress-%23117AC9.svg?style=for-the-badge&logo=WordPress&logoColor=white) ![Remix](https://img.shields.io/badge/remix-%23000.svg?style=for-the-badge&logo=remix&logoColor=white) ![C#](https://img.shields.io/badge/c%23-%23239120.svg?style=for-the-badge&logo=csharp&logoColor=white) ![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white) ![Ant-Design](https://img.shields.io/badge/-AntDesign-%230170FE?style=for-the-badge&logo=ant-design&logoColor=white) ![DigitalOcean](https://img.shields.io/badge/DigitalOcean-%230167ff.svg?style=for-the-badge&logo=digitalOcean&logoColor=white) ![AssemblyScript](https://img.shields.io/badge/assembly%20script-%23000000.svg?style=for-the-badge&logo=assemblyscript&logoColor=white) ![Apache Groovy](https://img.shields.io/badge/Apache%20Groovy-4298B8.svg?style=for-the-badge&logo=Apache+Groovy&logoColor=white) ![.Net](https://img.shields.io/badge/.NET-5C2D91?style=for-the-badge&logo=.net&logoColor=white) ![Express.js](https://img.shields.io/badge/express.js-%23404d59.svg?style=for-the-badge&logo=express&logoColor=%2361DAFB)
# 📊 GitHub Stats:
![](https://github-readme-stats.shion.dev/api?username=haseebqureshi611&theme=codeSTACKr&hide_border=false&include_all_commits=true&count_private=true)<br/>
![](https://streak-stats.demolab.com/?user=haseebqureshi611&theme=codeSTACKr&hide_border=false)<br/>
![](https://github-readme-stats.shion.dev/api/top-langs/?username=haseebqureshi611&theme=codeSTACKr&hide_border=false&include_all_commits=true&count_private=true&layout=compact)

### 🔝 Top Contributed Repo
![](https://github-contributor-stats.vercel.app/api?username=haseebqureshi611&limit=5&theme=dark&combine_all_yearly_contributions=true)

---
[![](https://komarev.com/ghpvc/?username=haseebqureshi611&icon=1&color=0)](https://visitcount.itsvg.in)

<!-- Proudly created with GPRM ( https://gprm.itsvg.in ) -->
