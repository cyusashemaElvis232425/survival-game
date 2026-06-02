<template>
  <div class="hud-bar">
    <div class="stat-box">
      <span class="label">SCORE</span>
      <span class="value score-text">{{ score }}</span>
    </div>
    
    <div class="stat-box">
      <span class="label">TIME SURVIVED</span>
      <span class="value">{{ timer }}s</span>
    </div>

    <div class="stat-box central">
      <span class="label">HEALTH</span>
      <div class="health-track">
        <div class="health-fill" :style="{ width: health + '%' }"></div>
      </div>
    </div>

    <div class="stat-box text-right">
      <span class="label">HI-SCORE: {{ highScore }}</span>
      <span class="value diff-text">STAGE {{ difficulty }}</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  score: Number,
  highScore: Number,
  health: Number,
  difficulty: Number,
  timer: Number
});
</script>

<style lang="scss" scoped>
//  with the Vite alias
@import '../assets/main.scss';

.hud-bar {
  @include retro-panel;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
}

.stat-box {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  min-width: 120px;

  &.central {
    flex-grow: 1;
    max-width: 350px;
    align-items: center;
  }
}

.label {
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  color: adjust-color($color-surface, $lightness: 40%);
}

.value {
  font-size: 1.5rem;
  font-weight: 800;
}

.score-text { color: $color-primary; }
.diff-text { color: #f59e0b; }

.health-track {
  width: 100%;
  height: 16px;
  background: #0f172a;
  border-radius: 4px;
  overflow: hidden;
  border: 1px solid #334155;
}

.health-fill {
  height: 100%;
  background: linear-gradient(90deg, $color-danger, #f43f5e);
  transition: width 0.1s ease-out;
}
</style>