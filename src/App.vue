<template>

<!-- logo -->
<p id="logo"><img src="./assets/logo.png" alt="Magic the Gathering logo" width="640" height="401"></p>

<section class="d-flex">

  <!-- search form listening for search submission -->
  <SearchForm @card-name-search="fetchCardImageUrls"/>
  
  <!-- loading animation -->
  <transition name="fade">
    <p v-show="loading">
      <img id="loading-circle" src="./assets/mana-circle.png" alt="cinq disques aux couleurs de magic disposés en cercle" width="220" height="220">
    </p>
  </transition>

</section>


<section>

  <!-- search result card count -->
    <div v-show="!loading">

      <p v-if="cardCount != 0">{{ cardCount }} cartes trouvées</p>
    
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
      currentPageInAPISearch: 0,
      totalNumberOfCardsInAPI: 0
    }
  },

  computed:{
    cardCount(){
      return this.cardImageUrls.length
    }
  },

  methods:{
    initSearch(){
      this.loading = true
      this.cardImageUrls = []
    },

    resetSearch(){
      this.loading = false
      this.currentPageInAPISearch = 0
    },

    fetchCardImageUrls(cardName){
      this.initSearch()
      do{
        this.currentPageInAPISearch ++;
        axios.get( 'https://api.magicthegathering.io/v1/cards?name='+cardName+'&page='+this.currentPageInAPISearch )
        .then( response => {
          this.totalNumberOfCardsInAPI = response.headers['total-count']
          this.cardImageUrls = this.cardImageUrls.concat(response.data.cards.map( card => card.imageUrl ).filter( url => url !== undefined ))
        })
      } while ( this.currentPageInAPISearch*100 < this.totalNumberOfCardsInAPI)
      this.resetSearch()
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

.fade-enter-from, .fade-leave-to{
  opacity: 0;
}

.fade-enter-active, .fade-leave-active{
  transition: opacity 0.25s;
}

#loading-circle{
  width: 60px;
  height: auto;
  margin-left: 10px;
  animation: rotation 2s infinite linear;
}

</style>
