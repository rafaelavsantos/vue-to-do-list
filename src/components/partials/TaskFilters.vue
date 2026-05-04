<template>
    <div class="d-flex gap-2 mb-3">
        <input v-model="search" type="text" placeholder="Busque uma tarefa" class="form-control" style="flex: 1;">
        <select v-model="status" class="form-select" style="flex: 1;">
            <option value="">Todas</option>
            <option value="pending">Pendentes</option>
            <option value="completed">Concluídas</option>
        </select>
        <button @click="cleanFilters" class="btn btn-outline-secondary btn-sm" style="flex-shrink: 0;">Limpar
            filtros</button>
    </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const search = ref('');
const status = ref('');

const emit = defineEmits(['search', 'status']);

const cleanFilters = () => {
  search.value = '';
  status.value = '';
}

watch(search, (vl) => {
    emit('search', vl);
});

watch(status, (vl) => {
    emit('status', vl);
});
</script>