<template>
  <div id="app">
    <img src="./assets/logo.png">
    <TodoList/>
  </div>
</template>

<script>
import TodoList from './components/TodoList'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
*{
  font: 1em sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  width: 600px;
  margin: 0 auto;
  text-align: center;
}
</style>
