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
  color: white;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

input {
  width: 100% !important;
}
</style>
