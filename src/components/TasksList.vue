<template>
    <ul
        class="todo-app__list"
    >
        <li
            v-for="(task, index) in tasks"
            :key="task.id"
            class="todo-app__item todo-app__item_draggable"
            :class="{
                'todo-app__item_opacity': index === dragElementIndex,
            }"
            draggable
            :data-index="index"
            @dragstart="dragElementIndex = index"
            @dragend="dragElementIndex = ''"
            @dragover.prevent="dragOver"
            @drop="dragDrop(index)"
        >
            <task-single 
                :task="task"
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
        },
        data() {
            return {
                dragElementIndex: '',
                dragEnterElementIndex: '',
            }
        },
        methods: {
            dragOver(event) {
                const currentElement = event.target.closest('li');
                const currentIndex = Number(currentElement.dataset.index);

                const isMoveable = this.dragElementIndex !== currentIndex;
                if (!isMoveable) {
                    return;
                }

                this.dragEnterElementIndex = currentIndex;

                const dragElem = this.$el.querySelector(`[data-index="${this.dragElementIndex}"]`)
                const nextElement = this.getNextElement(event.clientY, currentElement);

                if (nextElement && 
                    dragElem === nextElement.previousElementSibling ||
                    dragElem === nextElement
                ) {
                    return;
                }

                const list = this.$el;
                list.insertBefore(dragElem, nextElement);
            },
            dragDrop() {
                console.log(this.dragElementIndex, this.dragEnterElementIndex)
                if (this.dragElementIndex !== this.dragEnterElementIndex) {
                    this.$emit('sort-tasks', { draggableIndex: this.dragElementIndex, newIndex: this.dragEnterElementIndex });
                }
                this.dragEnterElementIndex = '';
                this.dragElementIndex = '';
            },
            getNextElement(cursorPosition, currentElement) {
                const currentElementCoord = currentElement.getBoundingClientRect();
                const currentElementCenter = currentElementCoord.y + currentElementCoord.height / 2;

                const nextElement = (cursorPosition < currentElementCenter) ?
                                    currentElement :
                                    currentElement.nextElementSibling;
  
                return nextElement;
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

        &__item {
            cursor: move;
            transition: all 200ms;

            &_opacity {
                opacity: .6;
            }

            &_over {
                transform: scale(1.1, 1.1);
            }
        }
    }
</style>