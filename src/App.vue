<template>
  <div id="app">
    <Users></Users>
  </div>
</template>

<script>
import Users from './components/Users'

export default {
  name: 'App',
  components: {
    Users
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  width: 800px;
  margin: 60px auto;
}
</style>
