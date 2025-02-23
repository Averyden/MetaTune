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

<style>
.audioPlayer {
  margin-top: 20px;
}
</style>
