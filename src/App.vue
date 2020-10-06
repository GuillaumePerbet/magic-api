<template>

<!-- logo -->
<p id="logo"><img src="./assets/logo.png" alt="Magic the Gathering logo" width="640" height="401"></p>

<!-- listen for card search and get images URL -->
<SearchForm @search-complete="getCardImageUrls"/>

<section>

  <!-- card search count -->
  <header>
    <p v-if="cardCount != 0">{{ cardCount }} cartes trouvées</p>
    <p v-else>Aucune carte trouvée</p>
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
      cardImageUrls: ['./assets/card-back.jpg']
    }
  },

  computed:{
    cardCount(){
      return this.cardImageUrls.filter( url => url !== './assets/card-back.jpg' ).length
    }
  },

  methods:{
    getCardImageUrls(payload){
      if (payload == []){
        this.cardImageUrls = ['./assets/card-back.jpg']
      } else {
        this.cardImageUrls = payload
      }
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

ul{
  display: flex;
  flex-wrap: wrap;
  list-style: none;
}

</style>
