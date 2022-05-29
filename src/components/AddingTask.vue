<template>
    <div class="todo-app__adding">
        <input 
            v-model="taskText"
            type="text"
            class="todo-app__input"
            placeholder="Вынести мусор"
            @keyup.enter="addTask"
        >
        <button 
            class="todo-app__btn"
            :disabled="!taskText"
            :title="taskText && 'Добавить'"
            @click="addTask"
        >
        </button>
    </div>
</template>
<script>
export default {
    data() {
        return {
            taskText: '',
        }
    },
    methods: {
        addTask() {
            if (!this.taskText) {
                return;
            }
            const task = {
                id: Math.random().toString(36).substr(2, 9),
                text: this.taskText,
                cheked: false,
            };
            this.$emit('add-task', task);
            this.taskText = '';
        }
    }
}
</script>
<style scoped lang="scss">
    .todo-app {

        &__adding {
            display: flex;
            align-items: center;
            position: relative;
        }

        &__input {
            background: #F9F1F1;
            box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
            border-radius: 6px;

            padding: 14px 16px;
            min-height: 42px;
            width: 100%;
            border: none;
            outline: none;
            box-sizing: border-box;

            // for text
            font-weight: 400;
            font-size: 15px;
            line-height: 18px;
            &::placeholder {
                color: rgba(#000000, .35);
            }
            color: #000000;
        }

        &__btn {

            position: absolute;
            display: block;
            top: 5px;
            bottom: 5px;
            right: 16px;
            width: 48px;
            background: #FED49B;
            background-image: url('../assets/icons/add.svg');
            background-repeat: no-repeat; 
            background-position: center;
            background-size: 20px;
            border-radius: 6px;
            outline: none;
            border: none;
            cursor: pointer;
            transition: all .3s ease;

            &:hover {
                background-color: darken(#FED49B, 10%);
            }

            &[disabled] {
                background-color: #e7e1e1;
                cursor: inherit;
            }
        }
    }
</style>