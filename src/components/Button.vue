<template>
    <button @click="onClick()"  :style={background:color} class="btn">{{ text }}</button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ButtonItem',
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick() {
      this.$emit('btn-click');
    },
  },
  emits: [
    'btn-click',
  ],
});
</script>
