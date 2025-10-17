<template>
  <div id="metadataViewer" v-if="metadata">
    <h2>Metadata</h2>
    <p><strong>Title:</strong> {{ metadata.title }}</p>
    <p><strong>Artist:</strong> {{ metadata.artist }}</p>
    <p><strong>Album:</strong> {{ metadata.album }}</p>
    <!-- <p><strong>Track:</strong> {{ metadata.track?.no }}</p> -->

    <!-- TODO: implement this button later :D -->

    <!-- <button @click="showAll = !showAll">{{ showAll ? 'Hide' : 'Show Full Metadata' }}</button>
    <pre v-if="showAll">{{ metadata }}</pre> -->

    <img v-if="metadata.coverUrl" :src="metadata.coverUrl" alt="Album Cover" class="albumCover" />
    <img v-else src="../assets/noIcon.png" alt="Placeholder Cover" class="albumCover" />
  </div>
</template>

<script setup lang="ts">
import type { Metadata } from '@/models/MetaData'

defineProps({
  metadata: {
    type: Object as () => Metadata,
    required: true,
  },
  visible: Boolean,
})
</script>

<style>
.metadata-viewer {
  margin-top: 30px;
  padding: 20px;
  border-radius: 12px;
  background-color: #e8f1f2;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  color: #2e4a4c;
  font-family: 'Roboto', sans-serif;
  text-align: left;
}

.albumCover {
  max-width: 100%;
  max-height: 250px;
  border-radius: 10px;
  margin-top: 15px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}
</style>
