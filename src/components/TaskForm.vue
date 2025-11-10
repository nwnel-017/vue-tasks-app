<script lang="ts" setup>
import { ref } from "vue";

const newTask = ref("");
const error = ref("");

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

function formSubmitted() {
  if (newTask.value.trim()) {
    emit("addTask", newTask.value.trim());
    newTask.value = "";
    error.value = "";
  } else {
    error.value = "Task cannot be empty!";
  }
}
</script>

<template>
  <form @submit.prevent="formSubmitted">
    <label>
      New Task
      <input v-model="newTask" name="newTask" clas="newTask" />
      <small v-if="error" class="error">{{ error }}</small>
    </label>
    <div class="buttonContainer">
      <button type="submit" class="submitBtn">Add Task</button>
    </div>
  </form>
</template>

<style scoped>
input {
  background-color: #333333ff;
  border: 1px solid #6ea3c9ff;
  border-radius: 0.25rem;
  color: var(--text-color);
  width: 100%;
  height: 2rem;
  padding: 0.5rem;
  margin: 0.5rem auto;
}
</style>
