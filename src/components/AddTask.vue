<template>
    <div class="todo-app__adding">
        <input 
            v-model="task"
            type="text"
            class="todo-app__input"
        >
        <button 
            class="todo-app__btn"
            :disabled="!task"
            @click="addTask"
        >
            Добавить
        </button>
    </div>
</template>
<script>
export default {
    data() {
        return {
            task: '',
        }
    },
    methods: {
        addTask() {
            this.$emit('add-task', this.task);
            this.task = '';
        }
    }
}
</script>
<style scoped lang="scss">
    .todo-app {

        &__adding {
            display: flex;
            align-items: center;
        }

        &__input {
            padding: 20px;
            min-height: 60px;
            max-width: 530px;
            width: 100%;
            border: 1px solid #787486;
            outline: none;
            filter: drop-shadow(0px 4px 4px rgba(0, 0, 0, 0.25));
            border-radius: 6px;
            box-sizing: border-box;

            // for text
            font-size: 14px;
            line-height: 18px;
            color: #0D062D;
        }

        &__btn {
            margin-left: 15px;
            padding: 15px;
            background: #4483F7;
            border-radius: 4px;
            outline: none;
            border: 1px solid #4483F7;
            cursor: pointer;
            transition: all .3s ease;

            // for text
            font-size: 16px;
            line-height: 19px;
            color: #FFFFFF;

            &[disabled] {
                border: 1px solid #999999;
                background-color: #cccccc;
                color: #666666;
                cursor: inherit;
            }
        }
    }
</style>