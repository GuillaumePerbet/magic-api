<template>

<!-- logo -->
<p id="logo"><img src="./assets/logo.png" alt="Magic the Gathering logo" width="640" height="401"></p>

<!-- search form listening for search submission -->
<SearchForm @search-complete="getCardImageUrls"/>

<section>

  <!-- search result card count -->
  <p v-if="cardCount != 0">{{ cardCount }} cartes trouvées</p>
  <p v-else>Aucune carte trouvée</p>

  <!-- cards showing search maching cards -->
  <Card :cardImageUrls="cardImageUrls"/>

</section>

</template>

<script>
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

  computed:{
    cardCount(){
      return this.cardImageUrls.length
    }
  },

  methods:{
    getCardImageUrls(payload){
      this.cardImageUrls = payload
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
