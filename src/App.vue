<template>
  <Header @card-name-search="fetchCardImageUrls"/>
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
    fetchCardImageUrls(cardName){
      axios.get( 'https://api.magicthegathering.io/v1/cards?name='+cardName )
      .then( response => {
        this.cardImageUrls = response.data.cards.map( card => card.imageUrl ).filter( url => url !== undefined )
      })
    }
  }
}
</script>

<style lang="scss">

</style>
