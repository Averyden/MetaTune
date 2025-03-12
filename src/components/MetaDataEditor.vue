<template>
  <div class="modal">
    <div class="modalContent">
      <h2>Editing Metadata</h2>
      <label>Title:</label>
      <input v-model="editableMetaData.title" type="text" />

      <label>Artist:</label>
      <input v-model="editableMetaData.artist" type="text" />

      <label>Album:</label>
      <input v-model="editableMetaData.album" type="text" />

      <div class="buttonGroup">
        <button class="btnSave" @click="saveChanges">Save</button>
        <button class="btnClose" @click="$emit('close')">Cancel</button>
      </div>
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

<style>
/* TODO: Stylize this like the notesu popup :D */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modalContent {
  background: white;
  padding: 20px;
  border-radius: 12px;
  text-align: center;
  width: 90%;
  max-width: 400px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

.modalContent h2 {
  color: black;
  font-size: 24px;
  margin-bottom: 10px;
}

label {
  display: block;
  text-align: left;
  font-weight: bold;
  margin: 10px 0 5px;
}

input {
  display: block;
  align-items: left;
  width: 95%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 16px;
  margin-bottom: 20px;
}

.buttonGroup {
  display: flex;
  justify-content: space-between;
  padding-top: 15px;
}

.btnSave {
  background: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: 0.3s;
}

.btnSave:hover {
  background: #45a049;
}

.btnClose {
  background: #e74c3c;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: 0.3s;
}

.btnClose:hover {
  background: #c0392b;
}
</style>
