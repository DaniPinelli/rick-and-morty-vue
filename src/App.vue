<template>
  <div id="app">
    <h1 class="title">Check Data</h1>
    <button v-on:click="fetch">Check</button>
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    fetch() {
      console.log("Hello World");
    },
  },
};
</script>

<style>
</style>
