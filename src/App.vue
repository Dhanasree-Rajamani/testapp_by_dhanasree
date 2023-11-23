<template>
  <div id="app">
    <h1>{{ message }}</h1>
    <button @click="changeMessage">Click Me</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      message: 'Hello Vue!'
    }
  },
  methods: {
    changeMessage() {
      this.message = 'You clicked the button!'
    }
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top: 60px;
}
button {
  font-size: 20px;
  padding: 10px 20px;
}
</style>

