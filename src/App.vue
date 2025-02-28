<template>
  <div id="app">
    <h1>MetaTune</h1>
    <div class="uploader-container">
      <AudioUploader @fileUploaded="handleFile" @uploadError="handleError" />
    </div>
    <p v-if="uploadedFile">Uploaded: {{ uploadedFile.name }}</p>
    <p v-if="error" class="errorMessage">{{ error }}</p>

    <AudioPlayer v-if="audioUrl" :src="audioUrl" />
    <MetaDataViewer/>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import AudioUploader from '@/components/AudioUploader.vue'
import AudioPlayer from './components/AudioPlayer.vue'
import MetaDataViewer from './components/MetaDataViewer.vue';

export default defineComponent({
  name: 'App',
  components: {
    AudioUploader,
    AudioPlayer,
    MetaDataViewer,
  },
  setup() {
    const uploadedFile = ref<File | null>(null)
    const audioUrl = ref<string | null>(null)
    const error = ref<string | null>(null)

    const handleFile = (file: File) => {
      uploadedFile.value = file
      audioUrl.value = URL.createObjectURL(file)
    }

    const handleError = (message: string) => {
      error.value = message
      uploadedFile.value = null
      audioUrl.value = null
    }

    return { uploadedFile, handleFile, audioUrl, error, handleError }
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
.errorMessage {
  color: red;
  font-size: 14px;
}
</style>
