<template>
  <div class="rate-star">
    <img v-bind:src="star === 1 ? fullStar :  star === 0.5 ? halfStar : emptyStar" alt="">
  </div>
</template>


<script>
import fullStar from "../assets/fullStar.png";
import halfStar from "../assets/halfStar.png";
import emptyStar from "../assets/emptyStar.png";
  
export default {
  props:['star'],
  data(){
    return{
      fullStar,
      halfStar,
      emptyStar
    }
  }
}
</script>