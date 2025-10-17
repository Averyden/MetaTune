<template>
  <div class="audioUploader">
    <input type="file" @change="handleFileUpload" accept="audio/*" />
  </div>
</template>

<script setup lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'AudioUploader',
  emits: ['fileUploaded', 'uploadError'],
  data() {
    return {
      errorMessage: '' as string,
    }
  },

  methods: {
    handleFileUpload(event: Event) {
      const target = event.target as HTMLInputElement
      const file = target.files?.[0]
      if (!file) return

      if (!file.type.startsWith('audio/')) {
        this.errorMessage = 'Please upload a valid audio file.'
        this.$emit('uploadError', this.errorMessage)
        return
      } else {
        this.errorMessage = ''
      }

      this.$emit('fileUploaded', file)
    },
  },
})
</script>

<style scoped>
.audioUploader {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  padding: 20px;
  background-color: var(--uploader);
  /* border: 2px dashed #7da8ab; */
  border-radius: 12px;
  transition: background-color 0.3s ease;
}

.audioUploader:hover {
  background-color: var(--uploader-hover);
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.05);
}

.audioUploader label {
  font-weight: 600;
  color: var(--text-dark);
  font-size: 16px;
}

input[type='file'] {
  background-color: var(--card-bg);
  color: var(--text-dark);
  border: none;
  padding: 10px 15px;
  border-radius: 8px;
  font-family: 'Roboto', sans-serif;
  font-size: 14px;
  cursor: pointer;
  width: 100%;
  max-width: 300px;
  transition: box-shadow 0.2s ease;
  transition:
    background-color,
    color,
    0.3s ease;
}

input[type='file']:hover {
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.15);
}

.audioUploader {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}
</style>
