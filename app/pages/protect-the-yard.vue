<script setup lang="ts">
const year = new Date().getFullYear();

import { onMounted } from "vue";

onMounted(() => {
  const defaultConfig = { game_title: "Lexi's Yard Dash" };
  let config = { ...defaultConfig };

  const HIGH_SCORE_KEY = "lexi-yard-dash-highscore";

  const obstacleTypes = [
    {
      svg: `<svg viewBox="0 0 50 50" class="w-full h-full">
        <ellipse cx="25" cy="30" rx="18" ry="15" fill="#6B7280"/>
        <ellipse cx="25" cy="18" rx="12" ry="10" fill="#6B7280"/>
        <circle cx="20" cy="16" r="3" fill="white"/>
        <circle cx="30" cy="16" r="3" fill="white"/>
        <circle cx="20" cy="16" r="1.5" fill="black"/>
        <circle cx="30" cy="16" r="1.5" fill="black"/>
        <ellipse cx="25" cy="22" rx="4" ry="3" fill="#1F2937"/>
        <polygon points="18,8 22,2 24,10" fill="#6B7280"/>
        <polygon points="28,10 30,2 34,8" fill="#6B7280"/>
        <rect x="8" y="35" width="5" height="15" rx="2" fill="#4B5563"/>
        <rect x="37" y="35" width="5" height="15" rx="2" fill="#4B5563"/>
        <path d="M 6 20 Q 0 25 6 30" stroke="#4B5563" stroke-width="3" fill="none"/>
      </svg>`,
      width: 45,
      height: 45,
    },
    {
      svg: `<svg viewBox="0 0 40 50" class="w-full h-full">
        <path d="M 30 10 Q 38 0 35 15 Q 40 25 35 35 Q 30 40 25 35" fill="#92400E"/>
        <ellipse cx="18" cy="30" rx="12" ry="14" fill="#B45309"/>
        <ellipse cx="15" cy="18" rx="8" ry="7" fill="#B45309"/>
        <circle cx="12" cy="16" r="2" fill="black"/>
        <ellipse cx="10" cy="20" rx="3" ry="2" fill="#7C2D12"/>
        <polygon points="10,10 14,5 16,12" fill="#B45309"/>
        <polygon points="18,12 22,5 20,10" fill="#B45309"/>
        <rect x="10" y="38" width="4" height="12" rx="1" fill="#92400E"/>
        <rect x="20" y="38" width="4" height="12" rx="1" fill="#92400E"/>
      </svg>`,
      width: 35,
      height: 45,
    },
    {
      svg: `<svg viewBox="0 0 40 60" class="w-full h-full">
        <polygon points="20,0 5,25 35,25" fill="#DC2626"/>
        <circle cx="20" cy="32" r="10" fill="#FECACA"/>
        <ellipse cx="20" cy="50" rx="12" ry="10" fill="#1E40AF"/>
        <circle cx="16" cy="30" r="2" fill="#1F2937"/>
        <circle cx="24" cy="30" r="2" fill="#1F2937"/>
        <ellipse cx="20" cy="36" rx="3" ry="2" fill="#FCA5A5"/>
        <path d="M 15 38 Q 20 42 25 38" stroke="#7C2D12" stroke-width="1.5" fill="none"/>
        <ellipse cx="20" cy="40" rx="6" ry="4" fill="white"/>
      </svg>`,
      width: 35,
      height: 55,
    },
  ];

  function initGame() {
    // DOM elements (ClientOnly may render after onMounted)
    const lexi = document.getElementById("lexi") as HTMLDivElement | null;
    const obstaclesContainer = document.getElementById(
      "obstacles",
    ) as HTMLDivElement | null;
    const collectiblesContainer = document.getElementById(
      "collectibles",
    ) as HTMLDivElement | null;
    const scoreDisplay = document.getElementById(
      "score",
    ) as HTMLDivElement | null;
    const highScoreDisplay = document.getElementById(
      "high-score",
    ) as HTMLDivElement | null;
    const burgerCountDisplay = document.getElementById(
      "burger-count",
    ) as HTMLSpanElement | null;
    const startScreen = document.getElementById(
      "start-screen",
    ) as HTMLDivElement | null;
    const gameOverScreen = document.getElementById(
      "game-over-screen",
    ) as HTMLDivElement | null;
    const gameContainer = document.getElementById(
      "game-container",
    ) as HTMLDivElement | null;
    const titleElement = document.getElementById(
      "game-title",
    ) as HTMLHeadingElement | null;
    const startBtn = document.getElementById(
      "start-btn",
    ) as HTMLButtonElement | null;
    const restartBtn = document.getElementById(
      "restart-btn",
    ) as HTMLButtonElement | null;

    if (
      !lexi ||
      !obstaclesContainer ||
      !collectiblesContainer ||
      !scoreDisplay ||
      !highScoreDisplay ||
      !burgerCountDisplay ||
      !startScreen ||
      !gameOverScreen ||
      !gameContainer ||
      !titleElement ||
      !startBtn ||
      !restartBtn
    ) {
      return false;
    }

    const gameState = {
      isPlaying: false,
      isJumping: false,
      score: 0,
      highScore: 0,
      burgerCount: 0,
      speed: 5,
      obstacleInterval: null as ReturnType<typeof setInterval> | null,
      collectibleInterval: null as ReturnType<typeof setInterval> | null,
      obstacles: [] as Array<{
        element: HTMLDivElement;
        width: number;
        height: number;
      }>,
      collectibles: [] as Array<{ element: HTMLDivElement }>,
    };

    // Restore high score
    try {
      const stored = localStorage.getItem(HIGH_SCORE_KEY);
      if (stored) {
        gameState.highScore = Number(stored) || 0;
        highScoreDisplay.textContent = String(gameState.highScore);
      }
    } catch {}

    // Apply title config
    titleElement.textContent = config.game_title || defaultConfig.game_title;

    function jump() {
      if (gameState.isJumping || !gameState.isPlaying) return;
      gameState.isJumping = true;
      lexi.classList.remove("lexi-running");
      lexi.classList.add("lexi-jumping");
      window.setTimeout(() => {
        lexi.classList.remove("lexi-jumping");
        lexi.classList.add("lexi-running");
        gameState.isJumping = false;
      }, 700);
    }

    function createObstacle() {
      const type =
        obstacleTypes[Math.floor(Math.random() * obstacleTypes.length)];
      const obstacle = document.createElement("div");
      obstacle.className = "absolute";
      obstacle.style.width = type.width + "px";
      obstacle.style.height = type.height + "px";
      obstacle.style.right = "-60px";
      obstacle.style.bottom = "0";
      obstacle.innerHTML = type.svg;
      obstaclesContainer.appendChild(obstacle);
      gameState.obstacles.push({
        element: obstacle,
        width: type.width,
        height: type.height,
      });
    }

    function createCollectible() {
      if (Math.random() > 0.4) return;
      const collectible = document.createElement("div");
      collectible.className = "absolute text-4xl";
      collectible.style.right = "-50px";
      collectible.style.bottom = Math.random() * 80 + 40 + "px";
      collectible.innerHTML = "🍔";
      collectiblesContainer.appendChild(collectible);
      gameState.collectibles.push({ element: collectible });
    }

    function checkCollision(a: DOMRect, b: DOMRect) {
      const padding = 10;
      return !(
        a.right - padding < b.left + padding ||
        a.left + padding > b.right - padding ||
        a.bottom - padding < b.top + padding ||
        a.top + padding > b.bottom - padding
      );
    }

    function persistHighScore() {
      try {
        localStorage.setItem(HIGH_SCORE_KEY, String(gameState.highScore));
      } catch {}
    }

    function gameOver() {
      gameState.isPlaying = false;
      gameContainer.classList.add("game-paused");

      if (gameState.obstacleInterval) clearInterval(gameState.obstacleInterval);
      if (gameState.collectibleInterval)
        clearInterval(gameState.collectibleInterval);

      const finalScore = document.getElementById("final-score");
      const finalBurgers = document.getElementById("final-burgers");
      finalScore && (finalScore.textContent = String(gameState.score));
      finalBurgers &&
        (finalBurgers.textContent = String(gameState.burgerCount));

      const newHigh = document.getElementById("new-high-score");
      if (gameState.score > gameState.highScore) {
        gameState.highScore = gameState.score;
        highScoreDisplay.textContent = String(gameState.highScore);
        newHigh && newHigh.classList.remove("hidden");
        persistHighScore();
      } else {
        newHigh && newHigh.classList.add("hidden");
      }

      gameOverScreen.style.display = "flex";
      gameOverScreen.classList.remove("hidden");
    }

    function loop() {
      if (!gameState.isPlaying) return;

      const lexiRect = lexi.getBoundingClientRect();

      gameState.obstacles = gameState.obstacles.filter((obs) => {
        const currentRight = parseInt(obs.element.style.right || "0", 10) || 0;
        obs.element.style.right = currentRight + gameState.speed + "px";
        const obsRect = obs.element.getBoundingClientRect();
        if (checkCollision(lexiRect, obsRect)) {
          gameOver();
          return false;
        }
        if (currentRight > window.innerWidth + 100) {
          obs.element.remove();
          gameState.score += 10;
          scoreDisplay.textContent = String(gameState.score);
          return false;
        }
        return true;
      });

      gameState.collectibles = gameState.collectibles.filter((col) => {
        const currentRight = parseInt(col.element.style.right || "0", 10) || 0;
        col.element.style.right = currentRight + gameState.speed + "px";
        const colRect = col.element.getBoundingClientRect();
        if (checkCollision(lexiRect, colRect)) {
          col.element.remove();
          gameState.burgerCount += 1;
          gameState.score += 25;
          burgerCountDisplay.textContent = String(gameState.burgerCount);
          scoreDisplay.textContent = String(gameState.score);
          return false;
        }
        if (currentRight > window.innerWidth + 100) {
          col.element.remove();
          return false;
        }
        return true;
      });

      if (gameState.score > 0 && gameState.score % 100 === 0) {
        gameState.speed = Math.min(12, 5 + Math.floor(gameState.score / 100));
      }

      requestAnimationFrame(loop);
    }

    function startGame() {
      gameState.isPlaying = true;
      gameState.isJumping = false;
      gameState.score = 0;
      gameState.burgerCount = 0;
      gameState.speed = 5;
      gameState.obstacles = [];
      gameState.collectibles = [];

      scoreDisplay.textContent = "0";
      burgerCountDisplay.textContent = "0";

      obstaclesContainer.innerHTML = "";
      collectiblesContainer.innerHTML = "";

      startScreen.classList.add("hidden");
      gameOverScreen.classList.add("hidden");
      gameOverScreen.style.display = "none";
      gameContainer.classList.remove("game-paused");

      if (gameState.obstacleInterval) clearInterval(gameState.obstacleInterval);
      if (gameState.collectibleInterval)
        clearInterval(gameState.collectibleInterval);

      gameState.obstacleInterval = window.setInterval(createObstacle, 1800);
      gameState.collectibleInterval = window.setInterval(
        createCollectible,
        2500,
      );

      requestAnimationFrame(loop);
    }

    // Buttons
    startBtn.addEventListener("click", startGame);
    restartBtn.addEventListener("click", startGame);

    // Input
    const onKey = (e: KeyboardEvent) => {
      if (e.code !== "Space") return;
      e.preventDefault();
      if (!gameState.isPlaying && !startScreen.classList.contains("hidden")) {
        startGame();
      } else if (gameState.isPlaying) {
        jump();
      }
    };

    const onClick = (e: MouseEvent) => {
      const target = e.target as HTMLElement | null;
      if (target?.closest("button")) return;
      if (!gameState.isPlaying && !startScreen.classList.contains("hidden")) {
        startGame();
      } else if (gameState.isPlaying) {
        jump();
      }
    };

    const onTouch = (e: TouchEvent) => {
      const target = e.target as HTMLElement | null;
      if (target?.closest("button")) return;
      e.preventDefault();
      if (!gameState.isPlaying && !startScreen.classList.contains("hidden")) {
        startGame();
      } else if (gameState.isPlaying) {
        jump();
      }
    };

    document.addEventListener("keydown", onKey);
    gameContainer.addEventListener("click", onClick);
    gameContainer.addEventListener("touchstart", onTouch, { passive: false });

    return true;
  }

  // Try until ClientOnly content exists
  let tries = 0;
  const tryInit = () => {
    tries += 1;
    const ok = initGame();
    if (ok) return;
    if (tries < 40) window.setTimeout(tryInit, 50);
  };

  tryInit();
});
</script>

<template>
  <main class="min-h-screen bg-[#fbf7f1] text-gray-800">
    <!-- HEADER -->
    <!-- HEADER -->
    <header
      class="sticky top-0 z-20 bg-transparent backdrop-blur border-b border-purple-100"
    >
      <div
        class="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between"
      >
        <NuxtLink to="/" class="font-serif text-xl text-purple-900">
          lexithe.dog
        </NuxtLink>

        <nav class="text-sm text-gray-700 flex gap-6">
          <NuxtLink to="/" class="hover:text-purple-800 transition"
            >Home</NuxtLink
          >
          <NuxtLink to="/about" class="hover:text-purple-800 transition"
            >About</NuxtLink
          >
        </nav>
      </div>
    </header>
    <!-- GAME (Yard Dash) -->
    <section id="yard-dash" class="bg-white py-16 px-6">
      <div class="max-w-6xl mx-auto">
        <div class="text-center mb-8">
          <div class="kicker">Mini Game</div>
          <h2 class="mt-3 font-serif text-4xl text-purple-900">
            Lexi’s Yard Dash
          </h2>
          <p class="mt-4 text-gray-700 text-lg max-w-3xl mx-auto">
            Tap/click or press <span class="font-semibold">Space</span> to jump.
            Avoid intruders. Collect cheeseburgers.
          </p>
        </div>

        <ClientOnly>
          <div
            id="game-container"
            class="relative w-full overflow-hidden rounded-3xl border border-purple-100 shadow-xl"
            style="
              height: min(72vh, 720px);
              background: linear-gradient(
                180deg,
                #87ceeb 0%,
                #b0e0e6 50%,
                #98d8c8 100%
              );
            "
          >
            <!-- Clouds -->
            <div class="absolute top-10 cloud" style="left: 20%">
              <svg width="80" height="40" viewBox="0 0 80 40">
                <ellipse
                  cx="30"
                  cy="25"
                  rx="25"
                  ry="15"
                  fill="white"
                  opacity="0.9"
                />
                <ellipse
                  cx="50"
                  cy="20"
                  rx="20"
                  ry="12"
                  fill="white"
                  opacity="0.9"
                />
                <ellipse
                  cx="25"
                  cy="18"
                  rx="15"
                  ry="10"
                  fill="white"
                  opacity="0.9"
                />
              </svg>
            </div>
            <div
              class="absolute top-20 cloud"
              style="left: 60%; animation-delay: -5s"
            >
              <svg width="60" height="30" viewBox="0 0 60 30">
                <ellipse
                  cx="20"
                  cy="18"
                  rx="18"
                  ry="12"
                  fill="white"
                  opacity="0.8"
                />
                <ellipse
                  cx="40"
                  cy="15"
                  rx="15"
                  ry="10"
                  fill="white"
                  opacity="0.8"
                />
              </svg>
            </div>
            <div
              class="absolute top-8 cloud"
              style="left: 40%; animation-delay: -10s"
            >
              <svg width="70" height="35" viewBox="0 0 70 35">
                <ellipse
                  cx="25"
                  cy="22"
                  rx="22"
                  ry="13"
                  fill="white"
                  opacity="0.85"
                />
                <ellipse
                  cx="45"
                  cy="18"
                  rx="18"
                  ry="11"
                  fill="white"
                  opacity="0.85"
                />
              </svg>
            </div>

            <!-- Title -->
            <h3
              id="game-title"
              class="game-font absolute top-4 left-1/2 -translate-x-1/2 text-3xl md:text-4xl text-amber-800 drop-shadow-lg z-20"
              style="
                text-shadow:
                  3px 3px 0 #fbbf24,
                  -1px -1px 0 #fff;
              "
            >
              Lexi's Yard Dash
            </h3>

            <!-- Score -->
            <div class="absolute top-4 right-4 z-20">
              <div
                class="score-font bg-white/90 rounded-xl px-4 py-2 shadow-lg border-4 border-amber-500"
              >
                <div class="text-amber-800 text-sm">SCORE</div>
                <div id="score" class="text-3xl font-bold text-amber-600">
                  0
                </div>
              </div>
            </div>

            <!-- High Score -->
            <div class="absolute top-4 left-4 z-20">
              <div
                class="score-font bg-white/90 rounded-xl px-4 py-2 shadow-lg border-4 border-purple-500"
              >
                <div class="text-purple-800 text-sm">BEST</div>
                <div id="high-score" class="text-2xl font-bold text-purple-600">
                  0
                </div>
              </div>
            </div>

            <!-- Cheeseburger counter -->
            <div class="absolute top-24 right-4 z-20">
              <div
                class="score-font bg-white/90 rounded-xl px-3 py-1 shadow-lg border-3 border-orange-400 flex items-center gap-2"
              >
                <span class="text-2xl">🍔</span>
                <span
                  id="burger-count"
                  class="text-xl font-bold text-orange-600"
                  >0</span
                >
              </div>
            </div>

            <!-- Game Area -->
            <div
              id="game-area"
              class="absolute bottom-0 left-0 right-0"
              style="height: 60%"
            >
              <!-- Ground -->
              <div
                class="ground-pattern absolute bottom-0 left-0 right-0 h-16 border-t-4 border-green-700"
              ></div>

              <!-- Fence in background -->
              <div
                class="absolute bottom-16 left-0 right-0 h-20 flex"
                style="opacity: 0.6"
              >
                <svg
                  class="w-full h-full"
                  preserveAspectRatio="none"
                  viewBox="0 0 400 80"
                >
                  <defs>
                    <pattern
                      id="fence"
                      x="0"
                      y="0"
                      width="40"
                      height="80"
                      patternUnits="userSpaceOnUse"
                    >
                      <rect
                        x="5"
                        y="10"
                        width="8"
                        height="70"
                        fill="#8B4513"
                        rx="2"
                      />
                      <rect
                        x="27"
                        y="10"
                        width="8"
                        height="70"
                        fill="#8B4513"
                        rx="2"
                      />
                      <rect x="0" y="25" width="40" height="6" fill="#A0522D" />
                      <rect x="0" y="50" width="40" height="6" fill="#A0522D" />
                      <polygon points="9,10 5,0 13,0" fill="#8B4513" />
                      <polygon points="31,10 27,0 35,0" fill="#8B4513" />
                    </pattern>
                  </defs>
                  <rect width="100%" height="100%" fill="url(#fence)" />
                </svg>
              </div>

              <!-- Lexi the Cane Corso -->
              <div
                id="lexi"
                class="absolute bottom-16 lexi-running"
                style="width: 80px; height: 70px; left: 28%"
              >
                <svg
                  viewBox="0 0 100 90"
                  class="w-full h-full"
                  style="overflow: visible"
                >
                  <ellipse cx="50" cy="55" rx="35" ry="25" fill="#D4A574" />
                  <rect
                    class="leg-back"
                    x="60"
                    y="65"
                    width="12"
                    height="25"
                    rx="4"
                    fill="#D4A574"
                  />
                  <ellipse cx="66" cy="88" rx="8" ry="4" fill="#C49A6C" />
                  <rect
                    class="leg-front"
                    x="28"
                    y="65"
                    width="12"
                    height="25"
                    rx="4"
                    fill="#D4A574"
                  />
                  <ellipse cx="34" cy="88" rx="8" ry="4" fill="#C49A6C" />
                  <ellipse
                    cx="85"
                    cy="45"
                    rx="6"
                    ry="4"
                    fill="#D4A574"
                    transform="rotate(-30 85 45)"
                  />
                  <ellipse cx="25" cy="55" rx="12" ry="18" fill="#E0B88A" />
                  <rect
                    x="15"
                    y="25"
                    width="25"
                    height="35"
                    rx="8"
                    fill="#D4A574"
                  />
                  <rect
                    x="12"
                    y="38"
                    width="28"
                    height="8"
                    rx="2"
                    fill="#9333EA"
                  />
                  <circle cx="26" cy="46" r="4" fill="#FFD700" />
                  <ellipse cx="22" cy="22" rx="20" ry="18" fill="#C49A6C" />
                  <ellipse
                    cx="22"
                    cy="18"
                    rx="14"
                    ry="8"
                    fill="#5D4D3D"
                    opacity="0.7"
                  />
                  <ellipse cx="22" cy="12" rx="14" ry="8" fill="#D4A574" />
                  <ellipse
                    cx="32"
                    cy="24"
                    rx="8"
                    ry="6"
                    fill="#D4A574"
                    opacity="0.9"
                  />
                  <ellipse
                    cx="12"
                    cy="24"
                    rx="8"
                    ry="6"
                    fill="#D4A574"
                    opacity="0.9"
                  />
                  <polygon points="5,8 12,0 15,12" fill="#6B5344" />
                  <polygon points="29,8 36,0 39,12" fill="#6B5344" />
                  <polygon points="8,8 12,3 13,10" fill="#C49A6C" />
                  <polygon points="31,8 35,3 36,10" fill="#C49A6C" />
                  <ellipse cx="8" cy="28" rx="10" ry="8" fill="#A67C52" />
                  <ellipse cx="2" cy="26" rx="4" ry="3" fill="#6B5344" />
                  <ellipse cx="18" cy="16" rx="5" ry="4" fill="white" />
                  <circle cx="17" cy="16" r="3" fill="#4A3728" />
                  <circle cx="16" cy="15" r="1" fill="white" />
                  <ellipse cx="6" cy="34" rx="4" ry="6" fill="#FF6B9D" />
                </svg>
              </div>

              <!-- Obstacles container -->
              <div
                id="obstacles"
                class="absolute bottom-16 left-0 right-0 h-32"
              ></div>
              <!-- Collectibles container -->
              <div
                id="collectibles"
                class="absolute bottom-16 left-0 right-0 h-48"
              ></div>
            </div>

            <!-- Start Screen -->
            <div
              id="start-screen"
              class="absolute inset-0 bg-black/50 flex items-center justify-center z-30"
            >
              <div
                class="bg-white/95 rounded-3xl p-8 text-center shadow-2xl border-4 border-amber-500 mx-4 max-w-md"
              >
                <div class="text-6xl mb-4">🐕</div>
                <h4 class="game-font text-3xl text-amber-800 mb-4">
                  Meet Lexi!
                </h4>
                <p class="score-font text-gray-700 mb-2">
                  The 150 lb Cane Corso who defends her yard!
                </p>
                <p class="score-font text-gray-600 mb-4 text-sm">
                  Jump over intruders 🦝🐿️ and collect cheeseburgers 🍔
                </p>
                <div class="bg-purple-100 rounded-xl p-4 mb-6">
                  <p class="score-font text-purple-800 font-bold">
                    TAP or press SPACE to jump!
                  </p>
                </div>
                <button
                  id="start-btn"
                  class="game-font bg-gradient-to-r from-amber-500 to-orange-500 hover:from-amber-600 hover:to-orange-600 text-white text-2xl px-8 py-4 rounded-2xl shadow-lg transform hover:scale-105 transition-all"
                >
                  START GAME
                </button>
              </div>
            </div>

            <!-- Game Over Screen -->
            <div
              id="game-over-screen"
              class="absolute inset-0 bg-black/50 hidden items-center justify-center z-30"
            >
              <div
                class="bg-white/95 rounded-3xl p-8 text-center shadow-2xl border-4 border-red-500 mx-4 max-w-md"
              >
                <div class="text-6xl mb-4">😢</div>
                <h4 class="game-font text-3xl text-red-600 mb-4">Game Over!</h4>
                <p class="score-font text-gray-700 mb-2">Lexi got caught!</p>
                <div class="bg-amber-100 rounded-xl p-4 mb-4">
                  <p class="score-font text-amber-800">
                    Score:
                    <span id="final-score" class="font-bold text-2xl">0</span>
                  </p>
                  <p class="score-font text-orange-600">
                    Burgers:
                    <span id="final-burgers" class="font-bold">0</span> 🍔
                  </p>
                </div>
                <div
                  id="new-high-score"
                  class="hidden bg-yellow-200 rounded-xl p-3 mb-4 border-2 border-yellow-400"
                >
                  <p class="game-font text-yellow-800 text-xl">
                    🎉 NEW HIGH SCORE! 🎉
                  </p>
                </div>
                <button
                  id="restart-btn"
                  class="game-font bg-gradient-to-r from-green-500 to-emerald-500 hover:from-green-600 hover:to-emerald-600 text-white text-2xl px-8 py-4 rounded-2xl shadow-lg transform hover:scale-105 transition-all"
                >
                  PLAY AGAIN
                </button>
              </div>
            </div>
          </div>
        </ClientOnly>
      </div>
    </section>

    <!-- FOOTER -->
    <footer class="py-10 text-center text-sm text-gray-500">
      © {{ year }} Lexi The Dog — All snacks monitored
    </footer>
  </main>
</template>

<style scoped>
.font-serif {
  font-family: "Playfair Display", serif;
}

.game-font {
  font-family: "Bangers", cursive;
}
.score-font {
  font-family: "Comic Neue", cursive;
}

@keyframes run {
  0%,
  100% {
    transform: translateY(0) scaleX(-1);
  }
  50% {
    transform: translateY(-3px) scaleX(-1);
  }
}
@keyframes legRunBack {
  0% {
    transform: rotateZ(0deg);
    transform-origin: 34px 65px;
  }
  25% {
    transform: rotateZ(-25deg);
    transform-origin: 34px 65px;
  }
  50% {
    transform: rotateZ(0deg);
    transform-origin: 34px 65px;
  }
  75% {
    transform: rotateZ(25deg);
    transform-origin: 34px 65px;
  }
  100% {
    transform: rotateZ(0deg);
    transform-origin: 34px 65px;
  }
}
@keyframes legRunFront {
  0% {
    transform: rotateZ(0deg);
    transform-origin: 66px 65px;
  }
  25% {
    transform: rotateZ(25deg);
    transform-origin: 66px 65px;
  }
  50% {
    transform: rotateZ(0deg);
    transform-origin: 66px 65px;
  }
  75% {
    transform: rotateZ(-25deg);
    transform-origin: 66px 65px;
  }
  100% {
    transform: rotateZ(0deg);
    transform-origin: 66px 65px;
  }
}
@keyframes jump {
  0% {
    transform: translateY(0) scaleX(-1);
  }
  50% {
    transform: translateY(-140px) scaleX(-1);
  }
  100% {
    transform: translateY(0) scaleX(-1);
  }
}
@keyframes cloudFloat {
  0% {
    transform: translateX(100%);
  }
  100% {
    transform: translateX(-100px);
  }
}
@keyframes grassMove {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: -50px 0;
  }
}

.lexi-running {
  animation: run 0.2s ease-in-out infinite;
}
.lexi-running .leg-back {
  animation: legRunBack 0.4s ease-in-out infinite;
}
.lexi-running .leg-front {
  animation: legRunFront 0.4s ease-in-out infinite;
}
.lexi-jumping {
  animation: jump 0.7s ease-out forwards;
}
.cloud {
  animation: cloudFloat 15s linear infinite;
}
.ground-pattern {
  background: repeating-linear-gradient(
    90deg,
    #4ade80 0px,
    #22c55e 25px,
    #4ade80 50px
  );
  animation: grassMove 0.3s linear infinite;
}
.game-paused .ground-pattern {
  animation-play-state: paused;
}
.game-paused .cloud {
  animation-play-state: paused;
}
</style>
