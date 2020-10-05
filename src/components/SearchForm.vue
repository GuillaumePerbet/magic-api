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
        currentPageInAPISearch: 0,
        totalNumberOfCardsInAPI: 0,
        cardImageUrls: []
      }
  },

  methods:{

    resetSearch(){
      this.loading = false
      this.currentPageInAPISearch = 0
      this.cardImageUrls = []
    },

    async fetchCardImageUrls(){
        this.loading = true
        do{
            this.currentPageInAPISearch ++
            await axios.get( 'https://api.magicthegathering.io/v1/cards?name='+this.cardName+'&page='+this.currentPageInAPISearch )
            .then( response => {
                this.totalNumberOfCardsInAPI = response.headers['total-count']
                this.cardImageUrls = this.cardImageUrls.concat(response.data.cards.map( card => card.imageUrl ).filter( url => url !== undefined ))
            })
        }
        while ( this.currentPageInAPISearch*100 < this.totalNumberOfCardsInAPI )
        this.$emit( 'search-complete' , this.cardImageUrls )
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