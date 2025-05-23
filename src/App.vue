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

    <div class="button-group" v-if="uploadedFile">
      <button @click="isEditing = true">Edit Metadata</button>
      <button @click="downloadUpdatedFile" :disabled="!updatedBlob">Download</button>
    </div>

    <MetaDataEditor
      v-show="isEditing"
      :metadata="metadata"
      :visible="isEditing"
      @save="updateMetadata"
      @close="isEditing = false"
    />

    <DarkModeToggle />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import * as mm from 'music-metadata'
import { ID3Writer } from 'browser-id3-writer'
import AudioUploader from '@/components/AudioUploader.vue'
import AudioPlayer from './components/AudioPlayer.vue'
import MetaDataViewer from './components/MetaDataViewer.vue'
import MetaDataEditor from './components/MetaDataEditor.vue'
import DarkModeToggle from './components/DarkModeToggle.vue'

export default defineComponent({
  name: 'App',
  components: {
    AudioUploader,
    AudioPlayer,
    MetaDataViewer,
    MetaDataEditor,
    DarkModeToggle,
  },

  setup() {
    const uploadedFile = ref<File | null>(null)
    const audioUrl = ref<string | null>(null)
    const error = ref<string | null>(null)
    const isEditing = ref(false)
    const updatedBlob = ref<Blob | null>(null)
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

        updatedBlob.value = file
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

      generateUpdatedFile()
    }

    const generateUpdatedFile = async () => {
      if (!uploadedFile.value) return

      const fileBuffer = await uploadedFile.value.arrayBuffer()

      const writer = new ID3Writer(fileBuffer)

      writer.setFrame('TIT2', metadata.value.title || '')
      writer.setFrame('TPE1', [metadata.value.artist || ''])
      writer.setFrame('TALB', metadata.value.album || '')
      writer.setFrame('TRCK', metadata.value.track?.no?.toString() || '')
      writer.setFrame('APIC', {
        description: 'Cover',
        data: await fetch(metadata.value.coverUrl || '').then((res) => res.arrayBuffer()),
        type: 0x03,
      })

      writer.addTag()

      updatedBlob.value = writer.getBlob()
    }

    const downloadUpdatedFile = () => {
      if (!uploadedFile.value || !updatedBlob.value) return

      const url = URL.createObjectURL(updatedBlob.value)
      const a = document.createElement('a')
      a.href = url
      a.download = `${uploadedFile.value.name}_new`
      a.click()
      URL.revokeObjectURL(url)
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
      generateUpdatedFile,
      updatedBlob,
      downloadUpdatedFile,
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

.button-group {
  margin-top: 20px;
  display: flex;
  gap: 12px;
  justify-content: center;
}

.errorMessage {
  color: red;
  font-size: 14px;
}
</style>
