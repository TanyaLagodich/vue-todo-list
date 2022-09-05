<template>
    <div class="todo-app">
        <h1 class="todo-app__title">
          todo list
        </h1>
        <adding-task 
            @add-task="addTask"
        />
        <tasks-list 
            :tasks="tasks"
            @sort-tasks="sortTasks"
            @save-task="saveTask"
            @delete-task="deleteTask"
        />
    </div>
</template>

<script>
import AddingTask from '@/components/AddingTask.vue';
import TasksList from '@/components/TasksList.vue';

export default {
    components: {
        AddingTask,
        TasksList,
    },
    data() {
        return {
            tasks: [],
        }
    },
    created() {
        if (localStorage.getItem('tasks')) {
            this.tasks = JSON.parse(localStorage.getItem('tasks'));
        }
    },
    methods: {
        sortTasks({ draggableIndex, newIndex }) {
            const draggableElem = this.tasks[draggableIndex];
            const enterDragElem = this.tasks[newIndex];
            console.log({ draggableIndex, draggableElem, newIndex, enterDragElem });
            this.$set(this.tasks, draggableIndex, enterDragElem);
            this.$set(this.tasks, newIndex, draggableElem);
            console.log(this.tasks);
            // this.tasks.splice(draggableIndex, 1);
            // this.tasks.splice(newIndex, 0, draggableElem);
            this.updateLocalStorage();
        },
        addTask(task) {
            this.tasks.push(task);
            this.updateLocalStorage();
        },
        saveTask({ task, index }) {
            this.tasks[index] = task;
            this.updateLocalStorage();
        },
        deleteTask(index) {
            this.tasks.splice(index, 1);
            this.updateLocalStorage();
        },
        updateLocalStorage() {
            localStorage.setItem('tasks', JSON.stringify(this.tasks));
        }
    }
}
</script>
<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap');

  body {
    height: 100vh;
    font-family: 'Inter';
    font-style: normal;
    font-weight: 500;
    background-color: #85B0D1;
  }

  .todo-app {
    max-width: 50%;
    margin: 0 auto;
    min-height: 80%;

    @media (max-width: 768px) {
      max-width: 85%;
    }

    &__title {
      text-transform: uppercase;
      font-weight: 600;
      font-size: 24px;
      line-height: 29px;
      color: #FEE59B;;
    }
  }
</style>