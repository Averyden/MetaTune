<template>
  <div class="audioPlayer">
    <audio controls :src="src"></audio>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'AudioPlayer',
  props: {
    src: {
      type: String,
      required: true,
    },
  },
})
</script>

<style scoped>
.audioPlayer {
  margin-top: 20px;
  padding: 16px;
  border-radius: var(--border-radius);
  background-color: var(--card-bg);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
  transition: background-color 0.5s ease;
}

audio {
  width: 100%;
  max-width: 100%;
  filter: grayscale(15%);
}
</style>
