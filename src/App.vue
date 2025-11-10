<script lang="ts" setup>
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import type { Task, TaskFilter } from "./types";
import TaskList from "./components/TaskList.vue";
import FilterButton from "./components/FilterButton.vue";

const message = ref("Tasks App");
const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>("all");

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? total + 1 : total), 0)
);

const filteredTasks = computed(() => {
  if (filter.value === "all") {
    return tasks.value;
  } else if (filter.value === "done") {
    return tasks.value.filter((task) => task.done);
  } else if (filter.value === "todo") {
    return tasks.value.filter((task) => !task.done);
  }
  return tasks.value;
});

function addTask(newTask: string) {
  tasks.value.push({ id: crypto.randomUUID(), title: newTask, done: false });
}

function toggleDone(id: string) {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.done = !task.done;
  }
}

function removeTask(id: string) {
  const index = tasks.value.findIndex((task) => task.id === id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
  }
}

function setFilter(value: TaskFilter) {
  filter.value = value;
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a Task to Get Started</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed</h3>
    <div v-if="tasks.length" class="buttonContainer">
      <FilterButton
        :currentFilter="filter"
        filter="all"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="todo"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="done"
        @set-filter="setFilter"
      />
    </div>
    <TaskList
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<style>
main {
  max-width: 50%;
  margin: 1rem auto;
}
/* input {
  background-color: #333333ff;
  border: 1px solid #6ea3c9ff;
  border-radius: 0.25rem;
  color: var(--text-color);
  width: 100%;
  height: 2rem;
  padding: 0.5rem;
  margin: 0.5rem auto;
} */
.submitBtn {
  display: flex;
  align-items: center;
  border-radius: 0.25rem;
  border: 1px solid #6ea3c9ff;
  background-color: #2794e2ff;
  color: var(--text-color);
}
.secondaryBtn {
  background-color: rgb(54, 52, 52);
  border: 1px solid #6ea3c9ff;
  color: #6ea3c9ff;
  border-radius: 0.25rem;
  cursor: pointer;
}
.buttonContainer {
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
}
.error {
  color: var(--error-color);
}
.taskItem {
  background-color: rgb(54, 52, 52);
  margin: 1rem auto;
  border-radius: 0.5rem;
  padding: 0.5em;
  width: 100%;
}
</style>
