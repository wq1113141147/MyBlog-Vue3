<template>
  <img class="avatar-container" :src="url" :style="{width:size+'px',height:size+'px'}" alt="">
</template>

<script>

export default {
  name: "Avatar",
  props:{
    url:{
      type:String,
    },
    size:{
      type:Number,
      default:150
    },
  },
}
</script>

<style scoped>
.avatar-container{
  border-radius: 50%;
  object-fit: cover;
  display: block;
}
</style>