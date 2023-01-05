<template>
    <button @click="onClick()" :style={background:color} class="btn">{{ text }}</button>
</template>

<script lang="ts">
export default {
  name: 'ButtonItem',
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick() {
      console.log('click');
    },
  },
};
</script>
