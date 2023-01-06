<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task tracker" :showAddTask="showAddTask" />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Header from './components/Header.vue';
import Tasks from './components/Tasks.vue';
import AddTask from './components/AddTask.vue';

export declare interface Task {
  id: number;
  text: string;
  day: string;
  reminder: boolean;
}

export default defineComponent({
  name: 'App',
  async created() {
    const tasks = await this.fetchTasks();
    this.tasks = tasks;
    console.log('Component has been created!', tasks);
  },
  unmounted() {
    console.log('Component has been destroyed!');
  },
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [] as Task[],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    addTask(task: any) {
      this.tasks = [...this.tasks, task];
    },
    toggleReminder(id: number) {
      this.tasks = this.tasks.map((task: Task) => (task.id === id
        ? {
          ...task,
          reminder: !task.reminder,
        }
        : task));
    },
    deleteTask(id: number) {
      if (window.confirm('Are you sure you want to delete the task?')) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();
      return data;
    },
    async fetchTask(id:number) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
  },
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
