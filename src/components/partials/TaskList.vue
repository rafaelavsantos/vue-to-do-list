<template>
    <ul class="list-group">
        <li class="list-group-item d-flex align-items-center gap-2" v-for="task in props.tasks" :key="task.id">
            <template v-if="task.state === 'show'">
                <input v-model="task.completed" type="checkbox" class="form-check-input">
                <span class="flex-grow-1" :class="task.completed ? 'text-decoration-line-through text-muted' : null">{{
                    task.name }}</span>
                <button class="btn btn-success btn-sm" @click="editTask(task)">Editar</button>
                <button class="btn btn-danger btn-sm" @click="confirmDeleteTask(task)">Excluir</button>
            </template>

            <template v-else-if="task.state === 'edit'">
                <input v-model="task._name" type="text" class="form-control form-control-sm">
                <button class="btn btn-success btn-sm" @click="saveTask(task)">Salvar</button>
                <button class="btn btn-outline-secondary btn-sm" @click="cancelTask(task)">Cancelar</button>
            </template>

            <template v-else-if="task.state === 'delete'">
                <span class="flex-grow-1">
                    <div class="fw-semibold">
                        {{ task.name }}
                    </div>
                    <div class="text-muted small">
                        Tem certeza que deseja remover?
                    </div>
                </span>
                <button class="btn btn-danger btn-sm" @click="removeTask(task)">Sim, excluir</button>
                <button class="btn btn-outline-secondary btn-sm" @click="cancelTask(task)">Cancelar</button>
            </template>
        </li>
    </ul>
</template>

<script setup>
const props = defineProps(['tasks']);
const emit = defineEmits(['delete']);

const editTask = (task) => {
    if (!Object.hasOwn(task, '_name')) {
        task._name = task.name;
    }

    task.state = 'edit';
}

const saveTask = (task) => {
    if (Object.hasOwn(task, '_name')) {
        task.name = task._name;
    }

    delete task._name;

    task.state = 'show';
}

const cancelTask = (task) => {
    task.state = 'show';
}

const confirmDeleteTask = (task) => {
    task.state = 'delete';
}

const removeTask = (task) => {
    emit('delete', task);
}
</script>