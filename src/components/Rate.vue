<template>
  <div class="rate">
    <Star v-for="(item, index) in rate" v-bind:key="index" v-bind:star="item"/>
  </div>
</template>

<script>
import Star from './Star'
export default {
  props: ['rate'],
  components:{
    Star,
  }
}
</script>