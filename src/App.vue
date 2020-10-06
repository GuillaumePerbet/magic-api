<template>

<!-- logo -->
<p id="logo"><img src="./assets/logo.png" alt="Magic the Gathering logo" width="640" height="401"></p>

<!-- listen for card search and get images URL -->
<SearchForm @search-complete="getCardImageUrls"/>

<section>

  <!-- card search count -->
  <header>
    <p v-if="cardCount == 0">Aucune carte trouvée</p>
    <p v-else-if="cardCount == 1">1 carte trouvée</p>
    <p v-else>{{ cardCount }} cartes trouvées</p>
  </header>

  <!-- cards showing -->
  <ul>
    <Card v-for="( cardImageUrl , index ) in cardImageUrls" :key="index" :cardImageUrl="cardImageUrl"/>
  </ul>
  

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
    cardBack(){
      return require('./assets/card-back.jpg')
    },

    cardCount(){
      return this.cardImageUrls.filter( url => url !== this.cardBack ).length
    }
  },

  methods:{
    getCardImageUrls(payload){
      if (payload.length){
        this.cardImageUrls = payload
      } else {
        this.cardImageUrls = [this.cardBack]
      }
    }
  },

  mounted(){
    this.cardImageUrls.push(this.cardBack)
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

ul{
  display: flex;
  flex-wrap: wrap;
  list-style: none;
}

</style>
