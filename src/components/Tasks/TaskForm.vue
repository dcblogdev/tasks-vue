<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
    addTask: [task: string]
}>()

const task = ref("")
const error = ref("")

function handleNewTask() {
  if (task.value.trim()) {
    emit("addTask", task.value)
    task.value = ""
    error.value = ""
  } else {
    error.value = "Please provide a valid value!"
    task.value = task.value.trim()
  }
}
</script>

<template>
  <form @submit.prevent="handleNewTask">

      <div>
        <label for="task">New Task</label>
        <input 
            id="task" 
            name="task" 
            type="text" 
            v-model="task" 
            @input="error = ''"
            :aria-invalid="!!error"
            aria-describedby="invalid-helper" 
        >
        <small v-if="error" id="invalid-helper">
          {{ error }}
        </small>
      </div>

      <button>Add</button>
   
  </form>
</template> 
