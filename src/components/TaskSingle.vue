<template>
    <div class="task">
        <input 
            v-if="editingTask"
            v-model="taskCopy.text"
            type="text"
        >
        <div v-else>
            <input 
                v-model="taskCopy.checked"
                type="checkbox" 
                :id="taskCopy.id" 
            />
            <label 
                :for="taskCopy.id"
                class="todo-app__label"
            >
                <span>
                    {{ taskCopy.text }}
                </span>
            </label>
        </div>
        <div>
            <button
                v-if="editingTask"
                class="task__button task__button_saving"
                @click="saveTask"
            >
            </button>
            <button
                v-else
                class="task__button task__button_editing"
                @click="editingTask = true"
            >
            </button>
            <button
                class="task__button task__button_deleting"
                @click="$emit('delete-task')"
            >	
            </button>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        task: {
            type: Object,
            required: true,
        }
    },
    data() {
        return {
            taskCopy: {},
            editingTask: false,
        }
    },
    created() {
        this.taskCopy = Object.assign({}, this.$props.task);
    },
    methods: {
        saveTask() {
            this.editingTask = false;
            this.$emit('save-task', this.taskCopy);
        }
    }
}
</script>
<style scoped lang="scss">
    .task {
        padding: 13px 11px;
        margin-bottom: 11px;
        background: #FFFFFF;
        border-radius: 6px;

        display: flex;
        align-items: center;
        justify-content: space-between;

        input[type="checkbox"] {
            display: none;

            &:checked {

                & + label {
                    color: gray;
                    transition: color .3s ease;

                    &::before {
                        content: '\2713';
                        display: flex;
                        align-items: center;
                        justify-content: center;
                        color: #000000;
                    }

                    span {
                        &:after {
                            width: 100%;
                            animation: crossout .3s ease;
                        }
                    }
                }
            }
        }

        label {
            position: relative;
            padding-left: 31px;
            cursor: pointer; 
            flex-grow: 1;

            &::before {
                content: '';
                position: absolute;
                left: 0;
                display: block;
                width: 18px;
                height: 18px;
                background: #D9D9D9;
                border-radius: 50px;
            }

            span {
                position: relative;
                &:after {
                    content: '';
                    position: absolute;
                    top: 50%;
                    left: 0;
                    display: block;
                    width: 0;
                    height: 1px;
                    background-color: #000000;
                }
            }
        }

        &__button {
            border: none;
            outline: none;
            background-color: transparent;
            background-repeat: no-repeat;
            background-position: center;
            background-size: 14px 12px;
            width: 33px;
            height: 18px;
            cursor: pointer;
            color: #000000;

            &:not(:last-child) {
                margin-right: 7px;
            }

            &_editing {
                background-image: url('../assets/icons/edit.svg');
            }

            &_saving {
                background-image: url('../assets/icons/save.svg');
            }

            &_deleting {
                background-image: url('../assets/icons/delete.svg');
            }
        }

    }

    @keyframes crossout {
        0% {
            width: 0;
        }
        100% {
            width: 100%;
        }
    }
</style>