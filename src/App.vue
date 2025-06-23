<script lang="ts" setup>
import { ref, computed } from "vue";
import TaskForm from "./components/TaskForm.vue";
import FilterButton from "./components/FilterButton.vue";
import TaskList from "./components/TaskList.vue";

import type { Task, TaskFilter } from "./types";


const message = ref("Tasks app");
const tasks = ref<Task[]>([]);
const totalDone = computed(() => tasks.value.reduce((total, task) => task.done? total + 1 : total, 0));
const filter = ref<TaskFilter>("all");

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

function toggleDone(taskId: string) {
  const task = tasks.value.find((task) => task.id === taskId);
  if (task) {
    task.done = !task.done;
  }
}

function removeTask(taskId: string) {
  const taskIndex = tasks.value.findIndex((task) => task.id === taskId);
  if (taskIndex !== -1) {
    tasks.value.splice(taskIndex, 1);
  }
}

function setFilter(value: TaskFilter) {
  filter.value = value;
}

const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "todo":
      return tasks.value.filter((task) => !task.done);
    case "done":
      return tasks.value.filter((task) => task.done);
  }
});
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <img src="./assets/study.png" />
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length"> Add a task to get started </h3>
    <h3 v-else>{{totalDone}} / {{tasks.length}} tasks completed</h3>
    <div class="button-container">
      <FilterButton :current-filter="filter" filter="all" @set-filter="setFilter"/>
      <FilterButton :current-filter="filter" filter="todo" @set-filter="setFilter"/>
      <FilterButton :current-filter="filter" filter="done" @set-filter="setFilter"/>
    </div>
    <TaskList :tasks="filteredTasks" @toggle-done="toggleDone" @remove-task="removeTask"/>
  </main>
</template>

<style>
body {
  padding: 1rem;
}
main {
  max-width: 800px;
  margin: 1rem auto;
}

.button-container {
  width: 100%;
  display: flex;
  justify-content: end;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
h1 {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 1rem;
}
img {
  display: block;
  margin: 0 auto;
  width: 30%;
}
</style>
