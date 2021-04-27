<template>
  <div class="payment-item">
    <img class="payment-img" :src=" item.show ? item.src : ''" alt="">
    <div class="payment-name"></div>
  </div>
</template>
<script>
export default {
  props:['item']
}
</script>