<template>
    <div class="task">
        <div
            v-if="editingTask"  
            class="task__input-wrap"
        >
            <input 
                v-model="taskCopy.text"
                type="text"
                class="task__edit-input"
                @keyup.enter="saveTask"
                @keyup.esc="cancelEditing"
            >
        </div>
        <div 
            v-else
             class="task__input-wrap"
        >
            <input 
                v-model="taskCopy.checked"
                type="checkbox" 
                :id="taskCopy.id" 
                @change="saveTask"
            />
            <label 
                :for="taskCopy.id"
            >
                <span>
                    {{ taskCopy.text }}
                </span>
            </label>
        </div>
        <div>
            <button
                v-if="editingTask && !taskCopy.checked"
                class="task__button task__button_saving"
                @click="saveTask"
            >
            </button>
            <button
                v-else-if="!taskCopy.checked"
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
        },
        cancelEditing() {
            this.editingTask = false;
            this.taskCopy.text = this.$props.task.text;
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

        &__input-wrap {
            flex-grow: 1;
            max-width: 82%;

            input[type="text"] {
                width: 100%;
                background: #F9F1F1;
                box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
                border-radius: 6px;
                box-sizing: border-box;
                border: none;
                outline: none;
                padding: 14px 16px;
                min-height: 42px;
            }
        }

        input[type="checkbox"] {
            display: none;

            &:checked {

                & + label {
                    color: gray;
                    transition: color .3s ease;

                    &::before {
                        display: flex;
                        align-items: center;
                        justify-content: center;
                        color: #000000;

                        background-image: url('../assets/icons/check.svg');
                        background-repeat: no-repeat;
                        background-size: 11px;
                        background-position: center;
                    }

                    span {
                        &:after {
                            width: 100%;
                            background-color: #000000;
                            animation: crossout .3s ease;
                        }
                    }
                }
            }
        }

        label {
            position: relative;
            padding-left: 31px;
            display: block;
            width: 100%;
            box-sizing: border-box;
            cursor: pointer; 

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
                &::after {
                    content: '';
                    position: absolute;
                    top: 50%;
                    bottom: 49%;
                    left: 0;
                    display: block;
                    width: 0;
                    background-color: transparent;
                }
            }
        }

        &__buttons {
            display: flex;
            align-items: center;
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