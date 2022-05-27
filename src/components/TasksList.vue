<template>
    <ul>
        <li
            v-for="(task, index) in tasks"
            :key="index"
        >
            {{ task }}
            <button
                @click="$emit('delete-task', index)"
            >
                Удалить
            </button>
        </li>
    </ul>
</template>
<script>
    export default {
        props: {
            tasks: {
                type: Array,
                required: true,
            },
        }
    }
</script>