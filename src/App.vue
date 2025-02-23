<template>
  <div id="app">
    <h1>MetaTune</h1>
    <div class="uploader-container">
      <AudioUploader @file-uploaded="handleFile" />
    </div>
    <p v-if="uploadedFile">Uploaded: {{ uploadedFile.name }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import AudioUploader from '@/components/AudioUploader.vue'
import AudioPlayer from './components/AudioPlayer.vue'

export default defineComponent({
  name: 'App',
  components: {
    AudioUploader,
  },
  setup() {
    const uploadedFile = ref<File | null>(null)

    const handleFile = (file: File) => {
      uploadedFile.value = file
    }

    return { uploadedFile, handleFile }
  },
})
</script>

<style>
#app {
  font-family: 'Roboto', sans-serif;
  text-align: center;
  margin-top: 20px;
}

.uploader-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
</style>
