<template>
    <ul
        class="todo-app__list"
    >
        <li
            v-for="(task, index) in tasks"
            :key="index"
            class="todo-app__item todo-app__item-draggable"
            @dragstart="dragStart"
        >
            <task-single 
                :task="task"
                @edit-task="$emit('edit-task', index)"
                @save-task="(task) => $emit('save-task', {  task, index })"
                @delete-task="$emit('delete-task', index)"
            />
        </li>
    </ul>
</template>
<script>
import TaskSingle from './TaskSingle.vue'
    export default {
        components: { 
            TaskSingle,
        },
        props: {
            tasks: {
                type: Array,
                required: true,
            },
        }
    }
</script>
<style scoped lang="scss">
    .todo-app {
        
        &__list {
            padding-left: 0;
            list-style-type: none;
        }
    }
</style>