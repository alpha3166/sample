<template>
  <div class="container" id="app">
    <RandomStringGenerator/>
  </div>
</template>

<script>
import RandomStringGenerator from './components/RandomStringGenerator.vue'

export default {
  name: 'app',
  components: {
    RandomStringGenerator
  }
}
</script>
