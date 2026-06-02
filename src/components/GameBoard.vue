<template>
  <div class="board-wrapper" ref="boardRef" @mousemove="handleMouseMove">
    <svg class="game-svg" viewBox="0 0 800 600">
      <defs>
        <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
          <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#1e293b" stroke-width="1"/>
        </pattern>
      </defs>
      <rect width="100%" height="100%" fill="url(#grid)" />

      <g v-for="hazard in hazards" :key="hazard.id">
        <circle :cx="hazard.x" :cy="hazard.y" :r="hazard.radius" fill="#ef4444" filter="drop-shadow(0px 0px 4px #ef4444)" />
        <circle :cx="hazard.x" :cy="hazard.y" :r="hazard.radius - 4" fill="#f87171" />
      </g>

      <g class="player-character" :transform="`translate(${playerX}, ${playerY})`">
        <circle cx="0" cy="0" r="15" fill="none" stroke="#38bdf8" stroke-width="2" />
        <polygon points="0,-12 10,10 -10,10" fill="#38bdf8" />
        <circle cx="0" cy="2" r="3" fill="#f8fafc" />
      </g>
    </svg>
  </div>
</template>

<script setup>
import { ref } from 'vue';

defineProps({
  hazards: Array
});

const emit = defineEmits(['update-player-pos']);
const boardRef = ref(null);

const playerX = ref(400);
const playerY = ref(300);

const handleMouseMove = (event) => {
  if (!boardRef.value) return;

  const rect = boardRef.value.getBoundingClientRect();
  
  // Calculate relative cursor positions matching viewports bounding box
  const scaleX = 800 / rect.width;
  const scaleY = 600 / rect.height;

  const localX = (event.clientX - rect.left) * scaleX;
  const localY = (event.clientY - rect.top) * scaleY;

  // Contain character perfectly inside SVG viewBox grid boundaries
  playerX.value = Math.max(15, Math.min(785, localX));
  playerY.value = Math.max(15, Math.min(585, localY));

  emit('update-player-pos', { x: playerX.value, y: playerY.value });
};
</script>

<style lang="scss" scoped>
//  with the Vite alias
@import '../assets/main.scss';

.board-wrapper {
  width: 100%;
  height: 600px;
  background: #0b0f19;
  border: 2px solid #334155;
  border-radius: 8px;
  cursor: none; // Hides default cursor pointer inside frame boundaries
  overflow: hidden;
}

.game-svg {
  width: 100%;
  height: 100%;
  display: block;
}
</style>