<template>
  <div id="app">
    <h1>MetaTune</h1>
    <AudioUploader @file-uploaded="handleFile" />
    <p v-if="uploadedFile">Uploaded: {{ uploadedFile.name }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import AudioUploader from '@/components/AudioUploader.vue'

export default defineComponent({
  name: 'App',
  components: {
    AudioUploader,
  },
  setup() {},
})
</script>
