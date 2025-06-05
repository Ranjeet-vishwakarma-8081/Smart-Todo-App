<script lang="ts" setup>

import { computed, ref } from 'vue';
import type { Task, TaskFilter } from './types';
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';
import FilterButton from './components/FilterButton.vue';

const message = ref("Smart Todo App");
const tasks = ref<Task[]>([]);
const filter =ref<TaskFilter>("all")

const totalDone = computed(()=>
  tasks.value.reduce((total,task)=>task.done ? total+1 : total, 0)
);

const filteredTasks = computed(()=>{
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "done":
      return tasks.value.filter((task)=>task.done);
    case "todo":
      return tasks.value.filter((task)=>!task.done);
    }
})

const addTask = (newTask:string)=>{
  tasks.value.push({
    id:crypto.randomUUID(),
    title: newTask,
    done: false
  })
}

const toggleDone = (id:string)=>{
  const task = tasks.value.find((task)=>task.id ===id);
  if(task)
    task.done=!task.done;
}
const removeTask=(id:string)=>{
  const index = tasks.value.findIndex((task)=>task.id ===id)
  if(index !== -1)
    tasks.value.splice(index, 1);
}

const setFilter = (value: TaskFilter)=>{
  filter.value = value;
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask"/>
    <h3 v-if="!tasks.length">Add a task to get started.</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed</h3>
    <div v-if="tasks.length" class="button-container">
      <FilterButton :currentFilter="filter" filter="all" @set-filter="setFilter"/>
      <FilterButton :currentFilter="filter" filter="todo" @set-filter="setFilter"/>
      <FilterButton :currentFilter="filter" filter="done" @set-filter="setFilter"/>
    </div>
    <TaskList :tasks="filteredTasks" @toggle-done="toggleDone" @remove-task="removeTask" />
  </main>
</template>

<style scoped>
main{
  max-width: 800px;
  margin: 1rem auto;
  padding: 12px;
}
.button-container{
  display: flex;
  justify-content: end;
  gap: 0.5rem;
}
</style>