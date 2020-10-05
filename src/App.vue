<template>

<!-- logo -->
<p id="logo"><img src="./assets/logo.png" alt="Magic the Gathering logo" width="640" height="401"></p>

<section class="d-flex">

  <!-- search form listening for search submission -->
  <SearchForm @card-name-search="fetchCardImageUrls"/>
  
  <p v-show="loading"><img id="loading-circle" src="./assets/mana-circle.png" alt="cinq disques aux couleurs de magic disposés en cercle" width="220" height="220"></p>

</section>


<section>

  <div v-if="search">
    <p v-if="cardCount != 0">Nombre de cartes trouvées: {{ cardCount }}</p>
  
    <p v-else>Aucune carte trouvée</p>
  </div>

  <!-- cards showing search maching cards -->
  <Card :cardImageUrls="cardImageUrls"/>

</section>

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
      cardImageUrls: [],
      loading: false,
      search: false
    }
  },

  computed:{
    cardCount(){
      return this.cardImageUrls.length
    }
  },

  methods:{
    fetchCardImageUrls(cardName){
      this.loading = true
      axios.get( 'https://api.magicthegathering.io/v1/cards?name='+cardName )
      .then( response => {
        this.loading = false
        this.search = true
        this.cardImageUrls = response.data.cards.map( card => card.imageUrl ).filter( url => url !== undefined )
      })
    }
  }
}
</script>

<style lang="scss">
.d-flex{
  display: flex;
  align-items: center;
}

#logo{
  width: 200px;

  img{
      width: 100%;
      height: auto;
  }
}

@keyframes rotation{
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}

#loading-circle{
  width: 60px;
  height: auto;
  margin-left: 10px;
  animation: rotation 2.5s infinite linear;
}

</style>
