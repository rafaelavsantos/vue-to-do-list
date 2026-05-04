<template>
    <div class="container" style="max-width: 800px;">
        <h1 class="text-center my-4">To-do List</h1>

        <!-- Stats -->
        <TaskStats :tasks="tasks" />

        <!-- Add new task -->
        <TaskAdd @add-task="addTask" />

        <!-- Filters -->
        <TaskFilters @search="onSearch" @status="onStatus" />

        <!-- Tasks -->
        <TaskList :tasks="filteredTasks" @delete="onDelete" />

        <!-- Empty state -->
        <TaskEmptyState v-if="filteredTasks.length === 0" :message="emptyStateMessage" />
    </div>
</template>

<script setup>
import { useStorage } from '@vueuse/core';
import TaskAdd from './partials/TaskAdd.vue';
import TaskStats from './partials/TaskStats.vue';
import { computed, ref } from 'vue';
import TaskFilters from './partials/TaskFilters.vue';
import TaskList from './partials/TaskList.vue';
import TaskEmptyState from './partials/TaskEmptyState.vue';

const tasks = useStorage('tasks', []);

// Se fosse manual manualmente deveria ser um watch
// const tasks = ref([])
//
// watch(tasks, (newTasks) => {
//  localStorage.setItem('tasks', JSON.stringify(newTasks))
// }, { deep: true })

const filterSearch = ref('');
const filterStatus = ref('');

const filteredTasks = computed(() => {
    let output = tasks.value;

    if (filterSearch.value) {
        const search = filterSearch.value.toLowerCase();

        output = output.filter(o => o.name.toLowerCase().includes(search));
    }

    if (filterStatus.value === 'pending') {
        return output.filter(o => o.completed === false);
    } else if (filterStatus.value === 'completed') {
        return output.filter(o => o.completed === true);
    }

    return output;
});

const onSearch = (search) => {
    filterSearch.value = search
};

const onStatus = (status) => {
    filterStatus.value = status
};

const addTask = (task) => {
    if (!task) { return }

    tasks.value.push({
        id: Date.now(),
        name: task,
        completed: false,
        state: 'show' // edit, delete
    })
};

const onDelete = (task) => {
    const index = tasks.value.findIndex(o => o.id === task.id)
    if (index !== -1) {
        tasks.value.splice(index, 1)
    }
};

const emptyStateMessage = computed(() => {
  let output = 'Nenhuma tarefa cadastrada.'

  if (filterSearch.value || filterStatus.value) {
    return 'Nenhum resultado para este filtro.'
  }

  return output;
});
</script>