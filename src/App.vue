<template>
  <div>
    <ul>
      <li v-for="item in items" :key="item">{{ item }}</li>
    </ul>
    <HelloWorld />
  </div>
</template>

<script>
  import HelloWorld from './components/HelloWorld.vue'

  export default {
    name: 'App',
    data() {
      return {
        items: [1, 2, 3]
      }
    },
    components: {
      HelloWorld
    }
  }
</script>

<style scoped lang="scss">
  #app {
    }
</style>