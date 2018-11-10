<template>
  <div id="app" class="wrapper">
    <img src="./assets/logo.png">
    <task-list></task-list>
  </div>
</template>

<script>
import TaskList from './components/TaskList';

export default {
  name: 'App',
  components: {
    TaskList,
  },
};
</script>

<style>

* {
  box-sizing: border-box;
}

.wrapper {
  max-width: 600px;
  margin: 0 auto;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 20px;
}
</style>
