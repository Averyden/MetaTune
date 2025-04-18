<template>
  <div class="audioUploader">
    <input type="file" @change="handleFileUpload" accept="audio/*" />
  </div>
</template>

<script lang="ts">
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
  background-color: #e8f1f2;
  border: 2px dashed #7da8ab;
  border-radius: 12px;
  transition: border-color 0.3s ease;
}

.audioUploader:hover {
  border-color: #558b92;
}

.audioUploader label {
  font-weight: 600;
  color: #2e4a4c;
  font-size: 16px;
}

input[type='file'] {
  background-color: #ffffff;
  border: none;
  padding: 10px 15px;
  border-radius: 8px;
  font-family: 'Roboto', sans-serif;
  font-size: 14px;
  cursor: pointer;
  width: 100%;
  max-width: 300px;
  transition: box-shadow 0.2s ease;
}

input[type='file']:hover {
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.15);
}

.audioUploader {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}
</style>
