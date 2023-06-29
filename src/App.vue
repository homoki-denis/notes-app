<script setup>
import { ref } from "vue";

// States
const showModal = ref(false);
const showError = ref(false);
const note = ref("");
const notes = ref([]);

// Get random Color
function getRandomColor() {
  var letters = "0123456789ABCDEF".split("");
  var color = "#";
  for (var i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}

// Add Notes
const addNotes = () => {
  if (note.value.trim().length < 9) {
    showError.value = true;
    return;
  }

  notes.value.push({
    text: note.value,
    date: new Date().toLocaleString([], {
      hour: "numeric",
      minute: "numeric",
      year: "numeric",
      month: "2-digit",
      day: "2-digit",
    }),
    bg: getRandomColor(),
    id: Math.floor(Math.random() * 1000000),
  });

  note.value = "";
  showModal.value = false;
  showError.value = false;
};
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model="note"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="showError" class="error">Please add more than 10 characters</p>
        <button @click="addNotes">Add Note</button>
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
          v-for="note in notes"
          class="card"
          :style="{ backgroundColor: note.bg }"
          :key="note.id"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  color: #fff;
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
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
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
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  margin: 0 1rem;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  background-color: rgb(193, 15, 15);
  margin-top: 7px;
}

.error {
  color: red;
}

@media screen and (max-width: 510px) {
  .cards-container {
    justify-content: center;
  }
}
</style>
