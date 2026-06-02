<template>
  <div class="overlay">
    <div class="card text-center">
      <h1 class="title">GAME OVER</h1>
      <p class="subtitle">You collapsed under the incoming pressure.</p>
      
      <div class="score-display">
        <span class="caption">FINAL PROTO-SCORE</span>
        <span class="digits">{{ score }}</span>
      </div>

      <button class="btn-restart" @click="$emit('restart')">
        RUN PROTOTYPE AGAIN
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  score: Number
});
defineEmits(['restart']);
</script>

<style lang="scss" scoped>
//  with the Vite alias
@import '../assets/main.scss';

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(15, 23, 42, 0.85);
  backdrop-filter: blur(4px);
  @include flex-center;
  border-radius: 8px;
  z-index: 10;
}

.card {
  @include retro-panel;
  max-width: 400px;
  width: 90%;
  text-align: center;
}

.title {
  color: $color-danger;
  font-size: 2.25rem;
  margin: 0 0 0.5rem 0;
  letter-spacing: 0.05em;
}

.subtitle {
    color: adjust-color($color-surface, $lightness: 30%);
  font-size: 0.9rem;
}

.score-display {
  background: rgba(15, 23, 42, 0.6);
  border: 1px solid #334155;
  padding: 1rem;
  border-radius: 6px;
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;

  .caption {
    font-size: 0.75rem;
    color: adjust-color($color-surface, $lightness: 40%);
  }
  .digits {
    font-size: 3rem;
    font-weight: 800;
    color: $color-primary;
  }
}

.btn-restart {
  background: $color-primary;
  color: $color-bg;
  border: none;
  font-weight: bold;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
  transition: all 0.2s ease;

  &:hover {
    background: adjust-color($color-primary, $lightness: 10%);
    box-shadow: 0 0 15px rgba(56, 189, 248, 0.6);
    transform: translateY(-1px);
  }
}
</style>