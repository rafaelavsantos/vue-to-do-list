<template>
    <div class="input-group mb-3">
        <input v-model="task" type="text" placeholder="Adicione uma nova tarefa" class="form-control"
            @keyup.enter="addTask">
        <button @click="addTask" class="btn btn-primary">Adicionar</button>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const task = ref('');

const emit = defineEmits(['add-task']);

const addTask = () => {
  if(!task.value) return;

  emit('add-task', task.value);
  
  task.value = '';
}

</script>