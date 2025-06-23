<script lang="ts" setup>
import { ref } from "vue";
const newTask = ref("");
const error = ref("");

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

function formSubmit() {
  if (newTask.value.trim()) {
    emit("addTask", newTask.value.trim());
    newTask.value = "";
  } else {
    error.value = "Task cannot be empty";
  }
}
</script>

<template>
  <form @submit.prevent="formSubmit">
    <label>
      New Task
      <input
        v-model="newTask"
        name="newTask"
        :aria-invalid="!!error || undefined"
        aria-describedby="invalid-helper"
        @input="error = ''"
      />
      <small v-if="error" id="invalid-helper">
        {{ error }}
      </small>
    </label>
    <div class="button-container">
      <button type="submit">Add Task</button>
    </div>
  </form>
</template>

<style scoped>
button{
  width: 20%;
  padding: 0.5rem;  
}

</style>
