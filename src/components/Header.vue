<template>
<header>
    <p><img src="../assets/logo.png" alt="Magic the Gathering logo" width="640" height="401"></p>

    <form @submit.prevent="cardNameSubmitted">
        <input id="cardName" name="cardName" type="text" v-model="cardName"><br/>
        <input type="submit" value="Rechercher une carte">
    </form>
</header>
</template>

<script>
export default {
  name: 'Header',

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
header{
    display: flex;
    align-items: center;
    margin: 20px;

    p{
        width: 200px;
        margin-right: 50px;

        img{
            width: 100%;
            height: auto;
        }
    }

    form{

        input{
            width: 200px;
            height: 30px;
            font-size: 18px;

            &#cardName{
                margin-bottom: 10px;
            }
        }
    }
}
</style>