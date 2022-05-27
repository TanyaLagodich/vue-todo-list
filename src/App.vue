<template>
    <div class="todo-app">
      <h1 class="todo-app__title">todo list</h1>
        <add-task 
            @add-task="addTask"
        />
        <tasks-list 
          :tasks="tasks"
          @delete-task="deleteTask"
        />
    </div>
</template>

<script>
import AddTask from '@/components/AddTask.vue';
import TasksList from '@/components/TasksList.vue';

export default {
  components: {
    AddTask,
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
    addTask(task) {
      this.tasks.push(task);
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
  }

  .todo-app {
    max-width: 50%;
    margin: 0 auto;
    background-color: #ffffff;
    min-height: 80%;

    &__title {
      text-align: center;
      text-transform: uppercase;
      font-size: 36px;
      line-height: 44px;
      color: #0D062D;
    }
  }
</style>