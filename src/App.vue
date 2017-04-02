<template>
  <div id="app">
    <div class="container">
      <viewer></viewer>
    </div>
  </div>
</template>

<script>
import Viewer from './components/Viewer.vue'

export default {
  name: 'app',
  components: {
    Viewer
  }
}
</script>

<style lang="scss" scoped>
#app {
  margin-top: 20px;
}
</style>
