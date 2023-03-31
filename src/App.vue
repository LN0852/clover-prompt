<template>
  <div class="app-container">
    <Prompt />
  </div>
</template>

<script>
import Prompt from './components/CloverPrompt.vue'

export default {
  name: 'App',
  components: {
    Prompt
  }
}
</script>

<style>
body {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  background-color: #f5f5f5;
  margin: 0;
}

.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
</style>