<script setup lang="ts">
import { ref } from "vue";
import TaskForm from "./TaskForm.vue";
import TaskList from "./TaskList.vue";
import type { Task } from "../../types";

const tasks = ref<Task[]>([]);

function store(task: string) {
  tasks.value.push({
      id: crypto.randomUUID(),
      title: task,
      completed: false,
    });
}

function clearTasks() {
    tasks.value = [];
}

function deleteTask(id: string) {
    tasks.value = tasks.value.filter((item) => item.id !== id);
}

function toggleTask(id: string) {
    tasks.value = tasks.value.map((item) => {
        if (item.id === id) {
            return { ...item, completed: !item.completed };
        }
        return item;
    });
}

</script>

<template>
    <TaskForm @add-task="store" />
    <h3 v-if="!tasks.length">Add a task</h3>
    <h3 v-else>{{ tasks.filter((item) => item.completed).length }} / ({{ tasks.length }}) tasks completed</h3>
    <TaskList
        :tasks
        @clear-tasks="clearTasks"
        @toggle-task="toggleTask"
        @delete-task="deleteTask"
    />
</template>
