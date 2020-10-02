<template>

<form @submit.prevent="cardNameSubmitted">
    <input id="cardName" name="cardName" type="text" v-model="cardName"><br/>
    <input type="submit" value="Rechercher une carte">
</form>

</template>

<script>
export default {
  name: 'SearchForm',

  data(){
      return {
          cardName: ''
      }
  },

  methods:{
      cardNameSubmitted(){
          this.$emit( 'card-name-search' , this.cardName )
      }
  }

}
</script>

<style lang="scss" scoped>

input{
    width: 200px;
    height: 30px;
    font-size: 18px;

    &#cardName{
        margin-bottom: 10px;
    }
}

</style>