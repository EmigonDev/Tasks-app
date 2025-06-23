<script lang="ts" setup>
    import type { Task } from "../types";

    const props = defineProps<{
        tasks: Task[];
    }>();

    const emits = defineEmits<{
        toggleDone : [taskId: string];
        removeTask: [taskId: string];
    }>();
</script>

<template>
    <TransitionGroup name="list" tag="div">
        <article v-for="task in props.tasks" :key="task.id" class="task">
            <label>
                <input type="checkbox" @input="emits('toggleDone', task.id)" :checked="task.done"/>
                <span :class="{ done: task.done }">{{ task.title }}</span>
            </label>
            <button class="outline" @click="emits('removeTask', task.id)">Remove</button>
        </article>
    </TransitionGroup>
</template>

<style>
.task{
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.5rem;
    border-bottom: 1px solid #ccc;
}

.done {
    text-decoration: line-through;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>