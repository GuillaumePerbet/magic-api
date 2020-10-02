<template>

  <div v-if="cardImageUrls.length" class="container">
    <p class="card" v-for="( cardImageUrl , index ) in cardImageUrls" :key="index" :style="backgroundImages[index]"></p>
  </div>

  <div v-else>
    <p class="card card-back"></p>
  </div>
  
</template>

<script>
export default {

  name: 'Card',

  props:{
    cardImageUrls: {
      type: Array,
      required: true
    }
  },

  computed:{
    backgroundImages(){
      // map url to { backgroundImage: url('url') } objects for style binding
      return this.cardImageUrls.map( url => { return {backgroundImage: "url('" + url + "')"} })
    }
  }

}
</script>

<style lang="scss" scoped>

.container{
  display: flex;
  flex-wrap: wrap;
}

.card{
  width: 254px;
  height: 352px;
  margin: 10px;
  background-size: cover;
  background-image: url('../assets/card-back.jpg');

  &-back{
    background-image: url('../assets/card-back.jpg');
  }
}

</style>
