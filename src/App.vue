<script setup>
import { ref } from 'vue';

const showModal = ref(false);
const newNote = ref('')
const notes = ref([])
const errorMessage = ref('')


function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return errorMessage.value = 'Notes should be atleast 10 characters long'
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  })
  showModal.value = false
  newNote.value = ''
  errorMessage = ''
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overley">
      <div class="modal">
        <h4>Write a Note</h4>
        <textarea v-model="newNote" name="note" id="note" cols="10" rows="5"></textarea>
        <p style="color: red" v-if="errorMessage">{{ errorMessage }}</p> <!-- if errormessage value is empty - false -->
        <div>
          <button @click="addNote">Add Note</button>
          <button @click="showModal = false" class="close">Close</button>
        </div>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="card-container">
        <div :key="note.id" v-for="note in notes" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
        </div>

      </div>
    </div>
  </main>
</template>


<style scoped>
main {
  width: 100vw;
  height: 100vh;
}

.container {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2.5rem;
}

header {
  display: flex;
  justify-content: space-between;
}

h1 {
  font-size: 75px;
  margin-bottom: 25px;
  font-weight: bold;
}

header button {
  border: none;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
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
  color: black;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
  align-self: flex-end;
}

.overley {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  border-radius: 12px;
  padding: 30px;
  background-color: #ffffff;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  background-color: chocolate;
  border: none;
  outline: none;
  border-radius: 6px;
  color: white;
  cursor: pointer;
  margin-top: 15px;
  margin-left: 15px;
}

.modal h4 {
  color: black;
  margin-bottom: 15px;
  font-size: 20px;
}

.modal textarea {
  border-radius: 6px;
  padding: 20px;
  font-size: 20px;
}

.modal div {
  display: flex;
  justify-content: end;
}

button.close {
  background-color: rebeccapurple;
}
</style>