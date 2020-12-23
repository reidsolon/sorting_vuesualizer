<template>
  <div id="app" class="">
      <Navigation></Navigation>
      <Content></Content>
  </div>
</template>

<script>
import Navigation from './components/Structure/Navigation'
import Content from './components/Structure/Content'
export default {
  name: 'App',
  components: {
    Navigation,
    Content
  }
}
</script>

<style>
#app {
}
</style>
