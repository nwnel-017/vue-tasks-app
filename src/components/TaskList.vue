<script lang="ts" setup>
import type { Task } from "../types";
import { TransitionGroup } from "vue";

const props = defineProps<{
  tasks: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
}>();
</script>

<template>
  <TransitionGroup name="task-list" tag="div" class="task-list">
    <article v-for="task in props.tasks" :key="task.id" class="taskItem">
      <label>
        <input
          @input="emits('toggleDone', task.id)"
          type="checkbox"
          class="checkBx"
          :checked="task.done"
        />
        <span :class="{ done: task.done }">{{ task.title }}</span>
      </label>
      <button class="secondaryBtn" @click="emits('removeTask', task.id)">
        Remove
      </button>
    </article>
  </TransitionGroup>
</template>

<style scoped>
label {
  cursor: pointer;
  display: flex;
  gap: 1rem auto;
}

.task-list {
  margin-top: 1rem;
}

.taskItem {
  display: flex;
  justify-content: space-between;
}

.checkBx {
  margin-right: 0.5rem;
  width: 1rem;
  height: 1rem;
  border-radius: 0.5rem;
  border: 1px solid #6ea3c9ff;
  background-color: #333333ff;
}

.done {
  text-decoration: line-through;
}

.task-list-enter-active,
.task-list-leave-active {
  transition: all 0.5s ease;
}
.task-list-enter-from,
.task-list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
