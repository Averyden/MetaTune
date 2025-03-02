<template>
  <div id="metadataViewer" v-if="metadata">
    <h2>Metadata</h2>
    <p><strong>Title:</strong> {{}}</p>
    <img src="../assets/placeholder.png" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'MetaDataViewer',
  props: {
    metadata: Object,
  },
  // TODO: Just slowly implement whatever over time
})
</script>
