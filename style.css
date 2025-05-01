const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const TILE_SIZE = 30;
const ROWS = canvas.height / TILE_SIZE;
const COLS = canvas.width / TILE_SIZE;

const themes = [
    { wall: 'darkgray', background: 'black', player: 'white', exit: 'green', coin: 'gold' },
    { wall: 'blue', background: 'black', player: 'green', exit: 'green', coin: 'yellow' },
    { wall: 'red', background: 'black', player: 'yellow', exit: 'green', coin: 'yellow' }
];
let currentTheme = 0;

const player = {
    x: 1,
    y: 1,
    size: TILE_SIZE - 4,
    color: themes[currentTheme].player,
};

const btn = document.getElementById('toggleInstructions');
    const instructions = document.getElementById('instructions');

    btn.addEventListener('click', () => {
        if (instructions.style.display === 'none') {
            instructions.style.display = 'block';
        } else {
            instructions.style.display = 'none';
        }
    });

const LIGHT_RADIUS = 2.8;

const exit = {
    x: COLS - 3,
    y: ROWS - 3,
    color: themes[currentTheme].exit
};

let coins = [
    { x: 4, y: 2, collected: false },
    { x: 7, y: 5, collected: false },
    { x: 10, y: 1, collected: false }
];

let maze;
let gameOver = false;
let collectedCoins = 0;

function generateMaze() {
    const maze = Array.from({ length: ROWS }, () => Array(COLS).fill(1));
    function carvePath(x, y) {
        const directions = [
            [0, -1], [0, 1], [-1, 0], [1, 0]
        ].sort(() => Math.random() - 0.5);
        for (const [dx, dy] of directions) {
            const nx = x + dx * 2;
            const ny = y + dy * 2;
            if (nx > 0 && ny > 0 && nx < COLS - 1 && ny < ROWS - 1 && maze[ny][nx] === 1) {
                maze[y + dy][x + dx] = 0;
                maze[ny][nx] = 0;
                carvePath(nx, ny);
            }
        }
    }
    maze[1][1] = 0;
    carvePath(1, 1);
    return maze;
}

if (!localStorage.getItem('maze')) {
    maze = generateMaze();
    localStorage.setItem('maze', JSON.stringify(maze));
} else {
    maze = JSON.parse(localStorage.getItem('maze'));
}

function placeCoins() {
    coins.forEach(coin => {
        while (maze[coin.y][coin.x] === 1) {
            coin.x = Math.floor(Math.random() * COLS);
            coin.y = Math.floor(Math.random() * ROWS);
        }
    });
}

placeCoins();

function drawMaze() {
    for (let y = 0; y < ROWS; y++) {
        for (let x = 0; x < COLS; x++) {
            if (maze[y][x] === 1) {
                ctx.fillStyle = themes[currentTheme].wall;
                ctx.fillRect(x * TILE_SIZE, y * TILE_SIZE, TILE_SIZE, TILE_SIZE);
            }
        }
    }
}

function drawExit() {
    ctx.fillStyle = exit.color;
    ctx.fillRect(exit.x * TILE_SIZE + 5, exit.y * TILE_SIZE + 5, TILE_SIZE - 10, TILE_SIZE - 10);
}

function drawCoins() {
    ctx.fillStyle = themes[currentTheme].coin;
    coins.forEach(coin => {
        if (!coin.collected) {
            ctx.beginPath();
            ctx.arc((coin.x + 0.5) * TILE_SIZE, (coin.y + 0.5) * TILE_SIZE, TILE_SIZE / 4, 0, Math.PI * 2);
            ctx.fill();
        }
    });
}

function drawPlayer() {
    ctx.fillStyle = player.color;
    ctx.fillRect(player.x * TILE_SIZE + 2, player.y * TILE_SIZE + 2, player.size, player.size);
}

function drawScore() {
    const scoreElement = document.getElementById('coinCounter');
    scoreElement.textContent = `Հավաքված կոպեկներ: ${collectedCoins}`;
}

function drawLighting() {
    ctx.fillStyle = 'rgba(0, 0, 0, 0.4)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    const gradient = ctx.createRadialGradient(
        (player.x + 0.5) * TILE_SIZE,
        (player.y + 0.5) * TILE_SIZE,
        0,
        (player.x + 0.5) * TILE_SIZE,
        (player.y + 0.5) * TILE_SIZE,
        LIGHT_RADIUS * TILE_SIZE
    );

    gradient.addColorStop(0, 'rgba(0, 0, 0, 0)');
    gradient.addColorStop(1, 'rgba(0, 0, 0, 1)');

    ctx.globalCompositeOperation = 'destination-out';
    ctx.fillStyle = gradient;
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.globalCompositeOperation = 'source-over';
}

function resetGame() {
    player.x = 1;
    player.y = 1;
    collectedCoins = 0;
    coins.forEach(coin => coin.collected = false);
    placeCoins();
    document.getElementById('nextLevelBtn').style.display = 'none';
    drawGame();
}

function movePlayer(dx, dy) {
    const newX = player.x + dx;
    const newY = player.y + dy;

    if (newX >= 0 && newY >= 0 && newX < COLS && newY < ROWS && maze[newY][newX] === 0) {
        player.x = newX;
        player.y = newY;
    }

    coins.forEach(coin => {
        if (coin.x === player.x && coin.y === player.y && !coin.collected) {
            coin.collected = true;
            collectedCoins++;
        }
    });

    const reachedExit = player.x === exit.x && player.y === exit.y;
    const allCollected = coins.every(coin => coin.collected);

    if (reachedExit && allCollected && !gameOver) {
        gameOver = true;
        alert(`🎉 Հաղթանակ! Դուք հավաքեցիք ${collectedCoins} կոպեկները և գտաք ավարտը!`);
        document.getElementById('nextLevelBtn').style.display = 'inline-block';
    }

    drawGame();
}

function startNewLevel() {
    gameOver = false;
    maze = generateMaze();
    localStorage.setItem('maze', JSON.stringify(maze));

    player.x = 1;
    player.y = 1;
    exit.x = COLS - 3;
    exit.y = ROWS - 3;

    coins.forEach(coin => {
        coin.collected = false;
        coin.x = Math.floor(Math.random() * COLS);
        coin.y = Math.floor(Math.random() * ROWS);
    });

    placeCoins();
    collectedCoins = 0;
    document.getElementById('nextLevelBtn').style.display = 'none';
    drawGame();
}

function drawGame() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawMaze();
    drawExit();
    drawCoins();
    drawPlayer();
    drawLighting();
    drawScore();
}

window.addEventListener('keydown', (event) => {
    switch (event.key) {
        case 'ArrowUp':
        case 'w':
            movePlayer(0, -1);
            break;
        case 'ArrowDown':
        case 's':
            movePlayer(0, 1);
            break;
        case 'ArrowLeft':
        case 'a':
            movePlayer(-1, 0);
            break;
        case 'ArrowRight':
        case 'd':
            movePlayer(1, 0);
            break;
        case 'r':
            resetGame();
            break;
        case 't':
            currentTheme = (currentTheme + 1) % themes.length;
            player.color = themes[currentTheme].player;
            exit.color = themes[currentTheme].exit;
            drawGame();
            break;
    }
});

document.getElementById('nextLevelBtn').addEventListener('click', () => {
    startNewLevel();
});

drawGame();
