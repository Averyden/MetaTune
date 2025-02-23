<template>
  <div class="audioUploader">
    <input type="file" @change="handleFileUpload" accept="audio/*" />
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'AudioUploader',
  emits: ['file-uploaded'],
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
        return
      }

      this.$emit('file-uploaded', file)
    },
  },
})
</script>
