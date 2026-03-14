<script setup lang="ts">
import { ref, computed } from "vue";
import TaskForm from "./TaskForm.vue";
import TaskList from "./TaskList.vue";
import type { Task } from "../../types";
import type { TaskFilter } from "../../types";

const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter >('all')
const pendingTasks = computed(() => tasks.value.filter((item) => !item.completed));
const completedTasks = computed(() => tasks.value.filter((item) => item.completed));

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
    <h3 v-else>{{ completedTasks.length }} / ({{ tasks.length }}) tasks completed</h3>
    <div v-if="tasks.length" class="button-container">
        <button :class="{ active: filter === 'all' }" class="outline" @click="filter = 'all'">All</button>
        <button :class="{ active: filter === 'pending' }" class="outline" @click="filter = 'pending'">Pending</button>
        <button :class="{ active: filter === 'completed' }" class="outline" @click="filter = 'completed'">Completed</button>
    </div>
    <TaskList
        :tasks="filter === 'all' ? tasks : filter === 'pending' ? pendingTasks : completedTasks"
        @clear-tasks="clearTasks"
        @toggle-task="toggleTask"
        @delete-task="deleteTask"
    />
</template>

<style>
.button-container {
    display: flex;
    gap: 8px;
    margin-bottom: 16px;
    justify-content:end;
    
}
</style>
