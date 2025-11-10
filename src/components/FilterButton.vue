<script lang="ts" setup>
import type { TaskFilter } from "../types";

const props = defineProps<{ filter: TaskFilter; currentFilter: TaskFilter }>();
const emits = defineEmits<{ setFilter: [filter: TaskFilter] }>();
</script>

<template>
  <button
    :class="{
      contrast: props.currentFilter === props.filter,
      secondaryBtn: true,
    }"
    @click="emits('setFilter', props.filter)"
  >
    {{ props.filter }}
  </button>
</template>

<style scoped>
button {
  text-transform: capitalize;
}
.contrast {
  background-color: #6ea3c9ff;
  color: #000000ff;
}
</style>
