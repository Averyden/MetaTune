<template>
  <div id="app">
    <h1>MetaTune</h1>
    <div class="uploader-container">
      <AudioUploader @file-uploaded="handleFile" />
    </div>
    <p v-if="uploadedFile">Uploaded: {{ uploadedFile.name }}</p>
    <AudioPlayer v-if="audioUrl" :src="audioUrl" />
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
    AudioPlayer,
  },
  setup() {
    const uploadedFile = ref<File | null>(null)
    const audioUrl = ref<string | null>(null)

    const handleFile = (file: File) => {
      uploadedFile.value = file
      audioUrl.value = URL.createObjectURL(file)
    }

    return { uploadedFile, handleFile, audioUrl }
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
