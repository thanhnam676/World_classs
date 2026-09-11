<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>Shooting Game</title>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background: #080b18;
      color: white;
      font-family: Arial, sans-serif;
      overflow: hidden;
      touch-action: none;
    }

    #gameContainer {
      position: relative;
      width: 100vw;
      height: 100vh;
    }

    canvas {
      display: block;
      width: 100%;
      height: 100%;
    }

    #ui {
      position: absolute;
      top: 15px;
      left: 15px;
      z-index: 10;
      font-size: 20px;
      line-height: 1.6;
      user-select: none;
    }

    #gameOver {
      position: absolute;
      inset: 0;
      background: rgba(0, 0, 0, 0.75);

      display: none;
      align-items: center;
      justify-content: center;
      flex-direction: column;

      z-index: 20;
    }

    #gameOver h1 {
      font-size: 50px;
      margin-bottom: 15px;
    }

    #gameOver p {
      font-size: 24px;
      margin-bottom: 20px;
    }

    #restartBtn {
      padding: 12px 25px;
      font-size: 20px;

      border: none;
      border-radius: 10px;

      background: #00c8ff;
      color: #001018;

      cursor: pointer;
      font-weight: bold;
    }

    #controls {
      position: absolute;
      bottom: 25px;
      left: 0;
      right: 0;

      display: flex;
      justify-content: space-between;

      padding: 0 25px;
      z-index: 10;
    }

    .controlGroup {
      display: flex;
      gap: 15px;
    }

    .controlBtn {
      width: 70px;
      height: 100px;

      border-radius: 50%;
      border: 2px solid rgba(255,255,255,0.6);

      background: rgba(255,255,255,0.15);
      color: white;

      font-size: 30px;
      font-weight: bold;

      user-select: none;
      -webkit-user-select: none;
    }

    #shootBtn {
      width: 85px;
      height: 85px;

      background: rgba(255, 50, 50, 0.35);
      border-color: #ff5555;
    }

    @media (min-width: 900px) {
      #controls {
        opacity: 0.35;
      }
    }
  </style>
</head>

<body>

<div id="gameContainer">

  <canvas id="game"></canvas>

  <div id="ui">
    <div>🎯 Điểm: <span id="score">0</span></div>
    <div>❤️ Máu: <span id="health">100</span></div>
  </div>

  <div id="gameOver">
    <h1>GAME OVER</h1>
    <p>Điểm của bạn: <span id="finalScore">0</span></p>ㄹ
    <button id="restartBtn">Chơi lại</button>
  </div>

  <div id="controls">

    <div class="controlGroup">
      <button class="controlBtn" id="leftBtn">←</button>
      <button class="controlBtn" id="rightBtn">→</button>
    </div>

    <button class="controlBtn" id="shootBtn">🔥</button>

  </div>

</div>

<script>

const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const scoreElement = document.getElementById("score");
const healthElement = document.getElementById("health");
const gameOverScreen = document.getElementById("gameOver");
const finalScoreElement = document.getElementById("finalScore");

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}

resizeCanvas();
window.addEventListener("resize", resizeCanvas);


// =========================
// GAME VARIABLES
// =========================

let score = 10;
let health = 100;
let gameRunning = true;

let bullets = [];
let enemies = [];
let particles = [];

const keys = {
  left: false,
  right: false
};


// =========================
// PLAYER
// =========================

const player = {
  x: canvas.width / 2,
  y: canvas.height - 120,

 
