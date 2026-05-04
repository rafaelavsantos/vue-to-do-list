<template>
    <div class="container" style="max-width: 800px;">
        <h1 class="text-center my-4">To-do List</h1>

        <!-- Stats -->
        <TaskStats :tasks="tasks" />

        <!-- Add new task -->
        <TaskAdd @add-task="addTask" />

    </div>
</template>

<script setup>
import { useStorage } from '@vueuse/core';
import TaskAdd from './partials/TaskAdd.vue';
import TaskStats from './partials/TaskStats.vue';

const tasks = useStorage('tasks', []);

const addTask = (task) => {
    if (!task) { return }

    tasks.value.push({
        id: Date.now(),
        name: task,
        completed: false,
        state: 'show' // edit, delete
    })
};


</script>