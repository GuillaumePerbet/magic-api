<template>

  <li :style="backgroundImage"></li>
  
</template>

<script>
export default {

  name: 'Card',

  props:{
    cardImageUrl: {
      type: String,
      required: true
    }
  },

  computed:{
    backgroundImage(){
      // map url to { backgroundImage: url('url') } objects for style binding
      return { backgroundImage: "url(" + this.cardImageUrl + ")" }
    }
  }

}
</script>

<style lang="scss" scoped>

li{
  width: 254px;
  height: 352px;
  margin: 10px;
  background-size: cover;
}

</style>
