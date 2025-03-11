<template>
  <div id="app">
    <h1>MetaTune</h1>
    <div class="uploader-container">
      <AudioUploader @fileUploaded="handleFile" @uploadError="handleError" />
    </div>
    <p v-if="uploadedFile">Uploaded: {{ uploadedFile.name }}</p>
    <p v-if="error" class="errorMessage">{{ error }}</p>

    <AudioPlayer v-if="audioUrl" :src="audioUrl" />
    <MetaDataViewer v-if="uploadedFile" :metadata="metadata" />

    <button v-if="uploadedFile" @click="isEditing = true">Edit Metadata</button>

    <MetaDataEditor
      v-if="isEditing"
      :metadata="metadata"
      @close="isEditing = false"
      @save="updateMetadata"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import * as mm from 'music-metadata'
import AudioUploader from '@/components/AudioUploader.vue'
import AudioPlayer from './components/AudioPlayer.vue'
import MetaDataViewer from './components/MetaDataViewer.vue'
import MetaDataEditor from './components/MetaDataEditor.vue'

export default defineComponent({
  name: 'App',
  components: {
    AudioUploader,
    AudioPlayer,
    MetaDataViewer,
    MetaDataEditor,
  },

  setup() {
    const uploadedFile = ref<File | null>(null)
    const audioUrl = ref<string | null>(null)
    const error = ref<string | null>(null)
    const isEditing = ref(false)
    const metadata = ref<{
      title?: string
      artist?: string
      album?: string
      track?: { no?: number; of?: number }
      coverUrl?: string
    }>({})

    const handleFile = async (file: File) => {
      try {
        error.value = null
        uploadedFile.value = file
        audioUrl.value = URL.createObjectURL(file)

        const extractedData = await mm.parseBlob(file)
        let coverUrl = null

        if (extractedData.common.picture && extractedData.common.picture.length > 0) {
          const picture = extractedData.common.picture[0]
          const blob = new Blob([new Uint8Array(picture.data)], { type: picture.format })
          coverUrl = URL.createObjectURL(blob)
        }

        metadata.value = {
          title: extractedData.common.title || 'Unknown Title',
          artist: extractedData.common.artist || 'Unknown Artist',
          album: extractedData.common.album || 'Unknown Album',
          track: {
            no: extractedData.common.track?.no ?? undefined,
            of: extractedData.common.track?.of ?? undefined,
          },
          coverUrl: coverUrl ?? '',
        }
      } catch (err) {
        error.value = 'Failed to parse metadata'
        uploadedFile.value = null
        audioUrl.value = null
        metadata.value = {}
      }
    }

    const handleError = (message: string) => {
      error.value = message
      uploadedFile.value = null
      audioUrl.value = null
    }

    const updateMetadata = (newMetadata: any) => {
      metadata.value = { ...metadata.value, ...newMetadata }
      isEditing.value = false
    }

    return {
      uploadedFile,
      handleFile,
      audioUrl,
      error,
      handleError,
      metadata,
      isEditing,
      updateMetadata,
    }
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
