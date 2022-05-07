<template>
  <div>
    <header>
      <h1>Todo App</h1>
    </header>
    <TodoApp />
    <footer>
      <ul>
        <li>z</li>
      </ul>
    </footer>
  </div>
</template>

<script>
  import TodoApp from './components/TodoApp.vue'

  export default {
    name: 'App',
    data() {
      return {}
    },
    components: {
      TodoApp
    }
  }
</script>

<style scoped lang="scss">
  h1 {
    margin: 50px 0 25px;
    text-align: center;
    font-size: 24px;
  }

  footer {
    width: 100%;

    ul {
      li {}
    }
  }
</style>