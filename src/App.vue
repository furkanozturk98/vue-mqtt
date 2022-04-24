<template>
  <div id="app">
    <div class="container mt-5">
    <div class="row">
      <div class="col-6">
        <publisher />
      </div>
    <div class="col-6">
      <subscriber />
    </div>
    </div>
    </div>
  </div>
</template>

<script>
import Publisher from './components/Publisher.vue'
import Subscriber from './components/Subscriber.vue'

export default {
  name: 'App',
  components: {
    Publisher,
    Subscriber
  }
}
</script>

<style>
/*#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}*/

</style>
