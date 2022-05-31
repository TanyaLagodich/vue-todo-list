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
                // получаем элемент, над которым находится переносимый элемент
                const currentElement = event.target.closest('li');
                const currentIndex = Number(currentElement.dataset.index);

                // если мы находимся над переносимым элементом
                // то ничего делать не нужно
                const isMoveable = this.dragElementIndex !== currentIndex;
                if (!isMoveable) {
                    return;
                }

                // переносимый элемент
                const dragElem = this.$el.querySelector(`[data-index="${this.dragElementIndex}"]`);
                // следующий элемент, после элемента, НАД которым переносим gragElem
                const nextElement = this.getNextElement(event.clientY, currentElement);

                // если nextElem и его предыдущий элемент это наш dragElem,
                // то ничего не делаем
                if (nextElement && 
                    dragElem === nextElement.previousElementSibling ||
                    dragElem === nextElement
                ) {
                    return;
                }

                // получаем весь список с элементами
                const list = this.$el;
                // вставляем над dragElemen перед nextELement
                list.insertBefore(dragElem, nextElement);

                // сохраняем index, на который поместим наш dragElem
                this.dragEnterElementIndex = currentIndex;
            },
            dragDrop() {
                if (this.dragEnterElementIndex && this.dragElementIndex !== this.dragEnterElementIndex) {
                    this.$emit('sort-tasks', { draggableIndex: this.dragElementIndex, newIndex: this.dragEnterElementIndex });
                }
                this.dragEnterElementIndex = '';
                this.dragElementIndex = '';
            },
            getNextElement(cursorPosition, currentElement) {
                // получаем координаты элемента, над которым переносим dragElem
                const currentElementCoord = currentElement.getBoundingClientRect();
                // получаем середину этого элемента по координатам Y + половина высоты элемента
                const currentElementCenter = currentElementCoord.y + currentElementCoord.height / 2;

                // если элемент пересек половину высоты элемента, то берем высоту следующего элемента
                // иначе берем элемент, над которым находимся
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