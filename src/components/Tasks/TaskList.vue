<script setup lang="ts">
import type { Task } from '../../types';

const props = defineProps<{
    tasks: Task[]
}>()

const emit = defineEmits<{
    deleteTask: [id: string]
    toggleTask: [id: string]
    clearTasks: []
}>()

</script>

<template>
  <div v-if="tasks.length > 0">
    <TransitionGroup name="list" tag="div">
        <article v-for="(task) in tasks" :key="task.id">
            <lable>
                <input type="checkbox" :checked="task.completed" @change="$emit('toggleTask', task['id'])" />
                <span :class="{ completed: task.completed }">{{ task['title'] }}</span>
            </lable>
        
            <button class="secondary" type="button" @click="$emit('deleteTask', task['id'])">Delete</button>
        </article>
    </TransitionGroup>
  
    <button type="button" @click="$emit('clearTasks')">Clear Tasks</button>

  </div>
</template> 

<style scoped>
.completed {
    text-decoration: line-through;
}

article {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

article button {
    font-size: 0.75rem;
    padding: 2px 6px;
}

.list-enter-active,
.list-leave-active {
    transition: all 0.3s ease;
}

.list-enter-from,
.list-leave-to {
    opacity: 0;
    transform: translateX(30px);
}
</style>
