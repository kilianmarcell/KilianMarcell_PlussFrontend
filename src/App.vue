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
    background: #222629;
    color: white;
  }
</style>