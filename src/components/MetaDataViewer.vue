<template>
  <div id="metadataViewer" v-if="metadata">
    <h2>Metadata</h2>
    <p><strong>Title:</strong> {{ metadata.title }}</p>
    <p><strong>Artist:</strong> {{ metadata.artist }}</p>
    <p><strong>Album:</strong> {{ metadata.album }}</p>
    <p><strong>Track:</strong> {{ metadata.track?.no }}</p>

    <!-- TODO: implement this button later :D -->

    <!-- <button @click="showAll = !showAll">{{ showAll ? 'Hide' : 'Show Full Metadata' }}</button>
    <pre v-if="showAll">{{ metadata }}</pre> -->

    <img v-if="metadata.coverUrl" :src="metadata.coverUrl" alt="Album Cover" class="albumCover" />
    <img v-else src="../assets/noIcon.png" alt="Placeholder Cover" class="albumCover" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'MetaDataViewer',
  props: ['metadata'],
})
</script>

<style>
.metadata-viewer {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  display: inline-block;
}
</style>
