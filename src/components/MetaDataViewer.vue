<template>
  <div id="metadataViewer" v-if="metadata">
    <h2>Metadata</h2>
    <p><strong>Title:</strong> {{ metadata.title }}</p>
    <p><strong>Artist:</strong> {{ metadata.artist }}</p>
    <p><strong>Album:</strong> {{ metadata.album }}</p>
    <p><strong>Track:</strong> {{ metadata.track?.no }}</p>

    <button @click="showAll = !showAll">{{ showAll ? 'Hide' : 'Show Full Metadata' }}</button>
    <pre v-if="showAll">{{ metadata }}</pre>
    <img src="../assets/placeholder.png" />
    <!--TODO: Find out if the metadata fetches it.-->
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'MetaDataViewer',
  props: {
    metadata: Object,
  },
  setup() {
    const showAll = ref(false)
    return { showAll }
  },
  // TODO: Just slowly implement whatever over time
})
</script>
