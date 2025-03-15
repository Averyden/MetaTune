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

@keyframes slide-up {
  0% {
    transform: translateY(0);
  }
  95% {
    opacity: 0;
  }
  100% {
    transform: translateY(-100%);
    visibility: hidden;
    opacity: 0;
  }
}

@keyframes slide-down {
  0% {
    transform: translateY(-100%);
    opacity: 0;
    visibility: visible;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

.modalContent {
  margin: auto;
  position: relative;
  background: #fff;
  border-radius: 10px;
  height: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  animation: slide-down 0.3s ease forwards;
  padding: 20px;
  width: 90%;
  max-width: 400px;
}

.modalContent h2 {
  font-family: 'Roboto', sans-serif;
  font-weight: bold;
  font-size: 25px;
  text-align: center;
  padding: 10px;
  color: black;
}

label {
  font-family: 'Roboto', sans-serif;
  color: #2e2e2e;
  display: block;
  text-align: left;
  font-weight: bold;
  margin: 10px 0 5px;
}

input {
  font-family: 'Roboto', sans-serif;
  color: #2e2e2e;
  background-color: #b0b0b0;
  font-size: 15px;
  padding: 16px;
  text-align: center;
  border-radius: 15px;
  width: 70%;
  height: 7%;
  resize: none;
  border: none;
  overflow: hidden;
}

.buttonGroup {
  display: flex;
  justify-content: space-between;
  padding-top: 15px;
}

.buttonGroup .btnSave,
.buttonGroup .btnClose {
  margin: 8px;
  min-width: 75px;
  width: auto;
  height: 30px;
  border: none;
  border-radius: 15px;
  font-family: 'Roboto', sans-serif;
  font-size: auto;
  transition: background 0.3s;
  cursor: pointer;
}

.btnSave {
  background-color: #f13ce8;
  color: #fff;
  transition: background 0.3s;
}

.btnSave:hover {
  background: #d823cf;
}
</style>
