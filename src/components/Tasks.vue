<template>
  <div :key="task.id" v-for="task in tasks">
    <Task
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @delete-task="$emit('delete-task', task.id)"
      :task="task"
    />
  </div>
</template>

<script lang="ts">
import { Task as TaskObj } from '@/App.vue';
import { defineComponent } from 'vue';
import Task from './Task.vue';

export default defineComponent({
  name: 'TasksItem',
  props: {
    tasks: {
      type: Array as () => Array<TaskObj>,
    },
  },
  components: {
    Task,
  },
  emits: ['delete-task', 'toggle-reminder'],
});
</script>
