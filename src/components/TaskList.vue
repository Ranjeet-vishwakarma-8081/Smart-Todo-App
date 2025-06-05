<script lang="ts" setup>
import type { Task } from '../types';

const props = defineProps<{
    tasks: Task[]
}>();

const emits = defineEmits<{
    toggleDone: [id:string]
    removeTask: [id:string]
}>()
</script>

<template>
    <TransitionGroup 
        class="task-list" 
        tag="div" 
        name="task-list">
        <article 
            class="task" 
            v-for="task in props.tasks" 
            :key="task.id"
            >
            <label>
                <input 
                    @input="emits('toggleDone', task.id)" 
                    :checked="task.done" 
                    type="checkbox"
                >
                    <span :class="{done: task.done}">
                        {{task.title}}
                    </span> 
            </label>
            <button 
                @click="emits('removeTask',task.id)" 
                class="outline"
            >
            Remove
            </button>
        </article>
    </TransitionGroup>
</template>

<style>
.task-list{
    margin-top: 1rem;
}
.done{
    text-decoration: line-through;
}
.task{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.task-list-enter-active,
.task-list-leave-active{
    transition: all 0.5s ease;
}
.task-list-enter-from,
.task-list-leave-to{
    opacity: 0;
    transform: translateX(300px);
}
</style>