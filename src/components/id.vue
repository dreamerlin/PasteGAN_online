<template>
  <div :id="id">
    <h1>{{id}}</h1>
    <!-- <img :src=imgUrl height="55px" width="55px"/> -->
  </div>
</template>

<script>
  export default{
    props:{
      id:{
        type:Number
      },
      key:{
        type:Number
      }
    },

    data: function() {
      imgUrl: '../assets/pic/num/' + this.$props.id + '.png'
    }
  };
</script>

<style>
</style>
