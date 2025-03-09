<template>
  <div class="modal">
    <div class="modalContent">
      <h2>Editing Metadata</h2>
      <label>Title:</label>
      <!--TODO: bind the v-model types properly thanks-->
      <input v-model="title" type="text" />

      <label>Artist:</label>
      <input v-model="artist" type="text" />

      <label>Album:</label>
      <input v-model="album" type="text" />

      <button>Save</button>
      <button>Cancel</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'

export default defineComponent({
  name: 'MetaDataEditor',
  props: ['metadata'],
  setup(props, { emit }) {
    const editableMetaData = ref({ ...props.metadata })

    watch(props.metadata, (newVal) => {
      editableMetaData.value = { ...newVal }
    })

    const saveChanges = () => {
      emit('save', editableMetaData.value)
    }

    return { editableMetaData, saveChanges }
  },
})
</script>
