<template>
  <Header></Header>
  <Card></Card>
</template>

<script>
import Header from './components/Header.vue'
import Card from './components/Card.vue'

export default {
  name: 'App',
  components: {
    Card,
    Header
  }
}
</script>

<style lang="scss">
#app {

}
</style>
