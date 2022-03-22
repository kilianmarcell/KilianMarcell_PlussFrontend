<template>
  <div id="page">
    <Pluss/>
  </div>
</template>

<script>
import Pluss from './components/Pluss.vue'

export default {
  name: 'App',
  components: {
    Pluss
  }
}
</script>

<style>
  #page {
    background: linear-gradient(120deg, #3a04ff, #fc0477, #ff0000);
    color: white;
  }
</style>