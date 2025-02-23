<template>
  <div class="audioUploader">
    <input type="file" @change="handleFileUpload" accept="audio/*" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'AudioUploader',
  emits: ['file-upload'],

  methods: {
    handleFileUpload(event: Event) {
      const target = event.target as HTMLInputElement
      const file = target.files?.[0]
      if (!file) return

      if (!file.type.startsWith('audio/')) {
        //TODO: make some sort of error message here.
      }
    },
  },
})
</script>
