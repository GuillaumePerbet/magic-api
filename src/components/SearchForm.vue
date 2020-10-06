<template>

<section class="d-flex">

  <!-- search card form -->
  <form @submit.prevent="fetchCardImageUrls">
    <input id="cardName" name="cardName" type="text" v-model="cardName"><br/>
    <input type="submit" value="Rechercher une carte">
  </form>
  
  <!-- loading animation -->
  <transition name="fade">
      <p v-show="loading">
        <img id="loading-circle" src="../assets/mana-circle.png" alt="cinq disques aux couleurs de magic disposés en cercle" width="220" height="220">
      </p>
  </transition>

</section>

</template>

<script>
import axios from 'axios'

export default {
  name: 'SearchForm',

  data(){
      return {
        cardName: '',
        loading: false,
        currentPage: 0,
        totalPages: 0,
        cardImageUrls: []
      }
  },

  computed:{
    filteredCardImageUrls(){
      // remove undefined URLs from cardImageUrls array
      return this.cardImageUrls.filter( url => url !== undefined )
    }
  },

  methods:{

    resetSearch(){
      // reset all search parameters
      this.loading = false
      this.currentPage = 0
      this.cardImageUrls = []
    },

    async fetchCardImageUrls(){
      // start loading animation
        this.loading = true
        do{
          // loop on request pages because API return pages of 100 items max
          this.currentPage ++
          // fetch current page of cardName
          await axios.get( 'https://api.magicthegathering.io/v1/cards?name='+this.cardName+'&page='+this.currentPage )
          .then( response => {
              // compute total number of pages
              this.totalPages = Math.trunc(response.headers['total-count']/100) + 1
              // add fetched URLs to cardImageUrls array
              this.cardImageUrls = this.cardImageUrls.concat(response.data.cards.map( card => card.imageUrl ))
          })
        }
        while ( this.currentPage < this.totalPages )
        // emit filtered array removing empty URLs
        this.$emit( 'search-complete' , this.filteredCardImageUrls )
        this.resetSearch()
    }
  }

}
</script>

<style lang="scss" scoped>

.d-flex{
  display: flex;
  align-items: center;
}

input{
    width: 200px;
    height: 30px;
    font-size: 18px;

    &#cardName{
        margin-bottom: 10px;
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