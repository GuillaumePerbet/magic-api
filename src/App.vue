<template>
  <Header @card-name-search="fetchCard"/>
  <main>
    <Card :cardImageUrls="cardImageUrls"/>
  </main>
</template>

<script>
import axios from 'axios'
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
      cardImageUrls: []
    }
  },

  methods:{
    fetchCard(cardName){
      axios.get( 'https://api.magicthegathering.io/v1/cards?name='+cardName )
      .then( response => {
        this.cardImageUrls = response.data.cards.map( card => card.imageUrl )
      })
    }
  }
}
</script>

<style lang="scss">

</style>
