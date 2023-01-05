<template>
    <div :key="task.id" v-for="task in tasks">
        <Task :task="task"/>
    </div>
</template>

<script lang="ts">
import Task from './Task.vue';

export default {
  name: 'TasksItem',
  props: {
    tasks: {
      type: Array as () => Array<{id:number, text:string, day:string, reminder:boolean}>,
    },
  },
  components: {
    Task,
  },
};
</script>
