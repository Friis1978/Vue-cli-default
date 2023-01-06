<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Task } from '@/App.vue';
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

export default defineComponent({
  name: 'HomePage',
  props: {
    showAddTask: Boolean,
  },
  async created() {
    const tasks = await this.fetchTasks();
    this.tasks = tasks;
    console.log('Component has been created!', tasks);
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [] as Task[],
    };
  },
  methods: {
    async addTask(task: Task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async toggleReminder(id: number) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      });

      const data = await res.json();

      this.tasks = this.tasks.map((task: Task) => (task.id === id
        ? {
          ...task,
          reminder: data.reminder,
        }
        : task));
    },
    async deleteTask(id: number) {
      if (window.confirm('Are you sure you want to delete the task?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        });

        if (res.status === 200) this.tasks = this.tasks.filter((task) => task.id !== id);
        else alert('Error deleting task');
      }
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();
      return data;
    },
    async fetchTask(id: number) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
  },
});
</script>
