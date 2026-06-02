<template>
  <div class="game-container">
    <GameHUD 
      :score="score" 
      :highScore="highScore" 
      :health="health" 
      :difficulty="difficulty" 
      :timer="elapsedSeconds"
    />
    
    <GameBoard 
      :hazards="hazards" 
      @update-player-pos="updatePlayerPosition" 
    />
    
    <GameOver 
      v-if="isGameOver" 
      :score="score" 
      @restart="restartGame" 
    />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue';
import GameHUD from './components/GameHUD.vue';
import GameBoard from './components/GameBoard.vue';
import GameOver from './components/GameOver.vue';

// Game Reactive State
const score = ref(0);
const highScore = ref(0);
const health = ref(100);
const difficulty = ref(1);
const elapsedSeconds = ref(0);
const isGameOver = ref(false);
const hazards = ref([]);

const playerPos = ref({ x: 400, y: 300 }); // SVG coordinates context
const boardDimensions = { width: 800, height: 600 };

const tickCounter = ref(0);
let primaryGameLoop = null;

const spawnHazard = () => {
  const edge = Math.floor(Math.random() * 4);
  let x, y;
  
  // Spawn just outside boundaries
  if (edge === 0) { x = Math.random() * boardDimensions.width; y = -20; }       // Top
  else if (edge === 1) { x = boardDimensions.width + 20; y = Math.random() * boardDimensions.height; } // Right
  else if (edge === 2) { x = Math.random() * boardDimensions.width; y = boardDimensions.height + 20; } // Bottom
  else { x = -20; y = Math.random() * boardDimensions.height; }                  // Left

  // Target trajectory points toward player's current location
  const angle = Math.atan2(playerPos.value.y - y, playerPos.value.x - x);
  const speed = 2 + difficulty.value * 1.5;

  hazards.value.push({
    id: Date.now() + Math.random(),
    x,
    y,
    vx: Math.cos(angle) * speed,
    vy: Math.sin(angle) * speed,
    radius: 12
  });
};

const updateGame = () => {
  if (isGameOver.value) return;

  hazards.value = hazards.value.reduce((acc, hazard) => {
    hazard.x += hazard.vx;
    hazard.y += hazard.vy;

    const dx = hazard.x - playerPos.value.x;
    const dy = hazard.y - playerPos.value.y;
    const distance = Math.sqrt(dx * dx + dy * dy);

    if (distance < hazard.radius + 15) {
      health.value = Math.max(0, health.value - 25);
      if (health.value <= 0) endGame();
      return acc;
    }

    if (hazard.x > -50 && hazard.x < boardDimensions.width + 50 &&
        hazard.y > -50 && hazard.y < boardDimensions.height + 50) {
      acc.push(hazard);
    }
    return acc;
  }, []);
};

const startLoop = () => {
  primaryGameLoop = setInterval(() => {
    if (isGameOver.value) return;

    tickCounter.value += 1;
    if (tickCounter.value >= 10) {
      tickCounter.value = 0;
      score.value += 1;
      elapsedSeconds.value += 1;
    }

    const nextDifficulty = Math.floor(elapsedSeconds.value / 10) + 1;
    if (nextDifficulty !== difficulty.value) {
      difficulty.value = nextDifficulty;
    }

    updateGame();

    const spawnChance = 0.08 + difficulty.value * 0.03;
    if (Math.random() < spawnChance) {
      spawnHazard();
    }
  }, 100);
};

const stopLoop = () => {
  if (primaryGameLoop) clearInterval(primaryGameLoop);
};

const updatePlayerPosition = (coords) => {
  if (isGameOver.value) return;
  playerPos.value = coords;
};

// Difficulty Progression Monitor
watch(elapsedSeconds, (seconds) => {
  const nextTier = Math.floor(seconds / 10) + 1;
  if (nextTier !== difficulty.value) {
    difficulty.value = nextTier;
  }
});

const endGame = () => {
  isGameOver.value = true;
  stopLoop();
  if (score.value > highScore.value) {
    highScore.value = score.value;
  }
};

const restartGame = () => {
  score.value = 0;
  health.value = 100;
  difficulty.value = 1;
  elapsedSeconds.value = 0;
  tickCounter.value = 0;
  hazards.value = [];
  isGameOver.value = false;
  startLoop();
};

onMounted(() => {
  startLoop();
});

onUnmounted(() => {
  stopLoop();
});
</script>

<style lang="scss">
//  with the Vite alias
@import './assets/main.scss';

.game-container {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  width: 800px;
  position: relative;
}
</style>