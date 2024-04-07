<script setup>
import { ref } from "vue";

let showModal = ref(false)
let newNote = ref("")
const errorMessage = ref("")
const notes = ref([])

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)"
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return errorMessage.value = "Need to be at least 10 characters long"
  } else {
    notes.value.push({
      id: Math.floor(Math.random() * 1000000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor()
    })
  }

  showModal.value = false
  newNote.value = ""
  errorMessage.value = ""
}

</script>

<template>
<main>
  <div v-if="showModal" class="overlay">
    <div class="modal">
      <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
      <p v-if="errorMessage">{{ errorMessage }}</p>
      <button @click="addNote">Add Note</button>
      <button @click="showModal = false" class="close">Close</button>
    </div>
  </div>
  <div class="container">
    <header>
      <h1>Notes</h1>
      <button @click="showModal = true">+</button>
    </header>
    <div class="cards-container">
      <div 
      class="card" 
      v-for="note in notes" 
      :key="note.id"
      :style="{ backgroundColor: note.backgroundColor }">
        <p class="main-text">{{ note.text }}</p>
        <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
      </div>
    </div>
  </div>
</main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 70px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  display: flex;
  align-items: center;
  justify-content: center;
  /* visibility: hidden; */
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: rgb(17, 158, 36);
  color: white;
  border: none;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  background-color: rgb(223, 95, 95);
}

.modal p {
  color: red;
}
</style>
