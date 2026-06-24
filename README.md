# pacman-jr-game
A Pac-Man Jr arcade game built with JavaScript
// Game constants
const TILE_SIZE = 20;
const CANVAS_WIDTH = 400;
const CANVAS_HEIGHT = 400;
const GRID_WIDTH = CANVAS_WIDTH / TILE_SIZE;
const GRID_HEIGHT = CANVAS_HEIGHT / TILE_SIZE;

// Game variables
let canvas = document.getElementById('gameCanvas');
let ctx = canvas.getContext('2d');
let score = 0;
let level = 1;
let gameRunning = true;
let gameSpeed = 5;

// Pac-Man object
let pacman = {
    x: 1,
    y: 1,
    vx: 0,
    vy: 0,
    radius: 8,
    mouthOpen: true
};

// Ghosts array
let ghosts = [
    { x: 8, y: 8, vx: 1, vy: 0, color: '#ff0000' },   // Red ghost
    { x: 9, y: 8, vx: -1, vy: 0, color: '#ffb8ff' },  // Pink ghost
    { x: 8, y: 9, vx: 0, vy: 1, color: '#00ffff' },   // Cyan ghost
    { x: 9, y: 9, vx: 0, vy: -1, color: '#ffba55' }   // Orange ghost
];

// Maze/walls (1 = wall, 0 = empty)
let maze = [
    [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],
    [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
    [1,0,1,1,0,1,1,1,1,0,0,1,1,1,1,0,1,1,0,1],
    [1,0,1,1,0,1,0,0,1,0,0,1,0,0,1,0,1,1,0,1],
    [1,0,0,0,0,0,0,1,1,0,0,1,1,0,0,0,0,0,0,1],
    [1,0,1,1,0,1,0,0,0,0,0,0,0,0,1,0,1,1,0,1],
    [1,0,1,1,0,1,1,1,0,1,1,1,1,1,1,0,1,1,0,1],
    [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
    [1,0,1,1,0,1,1,1,1,1,1,1,1,1,1,0,1,1,0,1],
    [1,0,1,1,0,1,0,0,0,0,0,0,0,0,1,0,1,1,0,1],
    [1,0,0,0,0,0,0,1,1,0,0,1,1,0,0,0,0,0,0,1],
    [1,0,1,1,0,1,0,0,1,0,0,1,0,0,1,0,1,1,0,1],
    [1,0,1,1,0,1,1,1,1,0,0,1,1,1,1,0,1,1,0,1],
    [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
    [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]
];

// Pellets (0 = no pellet, 1 = pellet)
let pellets = [];
initializePellets();

function initializePellets() {
    pellets = [];
    for (let y = 1; y < GRID_HEIGHT - 1; y++) {
        for (let x = 1; x < GRID_WIDTH - 1; x++) {
            if (maze[y][x] === 0) {
                pellets.push({ x, y, eaten: false });
            }
        }
    }
}

// Keyboard input
let keys = {};
document.addEventListener('keydown', (e) => {
    keys[e.key] = true;
    if (e.key === ' ') {
        gameRunning = !gameRunning;
        e.preventDefault();
    }
});

document.addEventListener('keyup', (e) => {
    keys[e.key] = false;
});

function handleInput() {
    let newVx = pacman.vx;
    let newVy = pacman.vy;

    if (keys['ArrowUp'] || keys['w']) newVy = -1, newVx = 0;
    if (keys['ArrowDown'] || keys['s']) newVy = 1, newVx = 0;
    if (keys['ArrowLeft'] || keys['a']) newVx = -1, newVy = 0;
    if (keys['ArrowRight'] || keys['d']) newVx = 1, newVy = 0;

    pacman.vx = newVx;
    pacman.vy = newVy;
}

function update() {
    if (!gameRunning) return;

    handleInput();

    // Update Pac-Man position
    let nextX = pacman.x + pacman.vx;
    let nextY = pacman.y + pacman.vy;

    if (isWalkable(nextX, nextY)) {
        pacman.x = nextX;
        pacman.y = nextY;
    }

    // Update ghosts
    ghosts.forEach((ghost, index) => {
        let nextGhostX = ghost.x + ghost.vx;
        let nextGhostY = ghost.y + ghost.vy;

        if (!isWalkable(nextGhostX, nextGhostY)) {
            // Change direction randomly
            let directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
            let randomDir = directions[Math.floor(Math.random() * directions.length)];
            ghost.vx = randomDir[0];
            ghost.vy = randomDir[1];
        } else {
            ghost.x = nextGhostX;
            ghost.y = nextGhostY;
        }
    });

    // Check pellet collision
    pellets.forEach((pellet) => {
        if (!pellet.eaten && pellet.x === pacman.x && pellet.y === pacman.y) {
            pellet.eaten = true;
            score += 10;
            document.getElementById('score').textContent = score;
        }
    });

    // Check ghost collision
    ghosts.forEach((ghost) => {
        if (pacman.x === ghost.x && pacman.y === ghost.y) {
            resetGame();
        }
    });

    // Check win condition
    if (pellets.every(p => p.eaten)) {
        level++;
        score += 100;
        document.getElementById('level').textContent = level;
        document.getElementById('score').textContent = score;
        initializePellets();
    }

    pacman.mouthOpen = !pacman.mouthOpen;
}

function isWalkable(x, y) {
    if (x < 0 || x >= GRID_WIDTH || y < 0 || y >= GRID_HEIGHT) return false;
    return maze[y][x] === 0;
}

function draw() {
    // Clear canvas
    ctx.fillStyle = '#000';
    ctx.fillRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);

    // Draw maze
    ctx.fillStyle = '#0080ff';
    for (let y = 0; y < GRID_HEIGHT; y++) {
        for (let x = 0; x < GRID_WIDTH; x++) {
            if (maze[y][x] === 1) {
                ctx.fillRect(x * TILE_SIZE, y * TILE_SIZE, TILE_SIZE, TILE_SIZE);
            }
        }
    }

    // Draw pellets
    ctx.fillStyle = '#ffb897';
    pellets.forEach((pellet) => {
        if (!pellet.eaten) {
            ctx.beginPath();
            ctx.arc(pellet.x * TILE_SIZE + TILE_SIZE / 2, pellet.y * TILE_SIZE + TILE_SIZE / 2, 2, 0, Math.PI * 2);
            ctx.fill();
        }
    });

    // Draw Pac-Man
    ctx.fillStyle = '#ffff00';
    ctx.beginPath();
    let mouthAngle = pacman.mouthOpen ? 0.3 : 0.1;
    let startAngle = mouthAngle;
    let endAngle = Math.PI * 2 - mouthAngle;
    ctx.arc(pacman.x * TILE_SIZE + TILE_SIZE / 2, pacman.y * TILE_SIZE + TILE_SIZE / 2, pacman.radius, startAngle, endAngle);
    ctx.lineTo(pacman.x * TILE_SIZE + TILE_SIZE / 2, pacman.y * TILE_SIZE + TILE_SIZE / 2);
    ctx.fill();

    // Draw ghosts
    ghosts.forEach((ghost) => {
        ctx.fillStyle = ghost.color;
        ctx.beginPath();
        ctx.arc(ghost.x * TILE_SIZE + TILE_SIZE / 2, ghost.y * TILE_SIZE + TILE_SIZE / 2, pacman.radius, 0, Math.PI * 2);
        ctx.fill();

        // Ghost eyes
        ctx.fillStyle = '#fff';
        ctx.beginPath();
        ctx.arc(ghost.x * TILE_SIZE + TILE_SIZE / 2 - 3, ghost.y * TILE_SIZE + TILE_SIZE / 2 - 2, 1.5, 0, Math.PI * 2);
        ctx.arc(ghost.x * TILE_SIZE + TILE_SIZE / 2 + 3, ghost.y * TILE_SIZE + TILE_SIZE / 2 - 2, 1.5, 0, Math.PI * 2);
        ctx.fill();
    });
}

function resetGame() {
    pacman.x = 1;
    pacman.y = 1;
    pacman.vx = 0;
    pacman.vy = 0;
    ghosts.forEach((ghost, index) => {
        ghost.x = 8 + (index % 2);
        ghost.y = 8 + Math.floor(index / 2);
    });
}

function gameLoop() {
    update();
    draw();
    setTimeout(gameLoop, 1000 / gameSpeed);
}

// Start the game
gameLoop();
