<script setup lang="ts">
import { ref } from 'vue';

const newTask = ref("");
const error = ref("");

const emit = defineEmits<{
    addTask: [newTask:string] 
  }>();

const handleSubmitted =()=>{
  if(newTask.value.trim()){
    emit('addTask',newTask.value.trim());  
    newTask.value = "";
  }
  else{
    error.value = "Tasks can't be empty!";
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmitted">
    <label>
      New Task
      <input 
        v-model="newTask" 
        name="newTask" 
        @input="error = ''"
        :aria-invalid="!!error || undefined"
        aria-describedby="invalid-helper"
      > 
      <small 
        v-if="error" 
        id="invalid-helper"
      >
        {{ error}}
      </small>
    </label>
    <div class="button-container">
      <button>Add</button>
    </div>
  </form>
</template>

<style scoped>

.button-container{
  display: flex;
  justify-content: end;
}
</style>