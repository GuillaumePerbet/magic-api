<template>
  <Header @card-name-search="fetchCard"/>
  <main>
    <Card v-bind="card"/>
  </main>
</template>

<script>
import Header from './components/Header.vue'
import Card from './components/Card.vue'

export default {

  name: 'App',

  components: {
    Card,
    Header
  },

  data(){
    return {
      card: {
        name: '',
        text: ''
      }
    }
  },

  methods:{
    fetchCard(cardName){
      this.card.name=cardName
    }
  }
}
</script>

<style lang="scss">
#app {

}
</style>
