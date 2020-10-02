<template>

<!-- logo -->
<p id="logo"><img src="./assets/logo.png" alt="Magic the Gathering logo" width="640" height="401"></p>

<!-- search form listening for search submission -->
<SearchForm @card-name-search="fetchCardImageUrls"/>

<!-- cards showing search maching cards -->
<Card :cardImageUrls="cardImageUrls"/>

</template>

<script>
import axios from 'axios'
import SearchForm from './components/SearchForm.vue'
import Card from './components/Card.vue'

export default {

  name: 'App',

  components: {
    Card,
    SearchForm
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

#logo{
  width: 200px;

  img{
      width: 100%;
      height: auto;
  }
}

</style>
