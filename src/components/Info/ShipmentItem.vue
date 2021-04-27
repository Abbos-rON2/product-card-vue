<template>
  <div class="shipment-item">
    <img class="shipment-img" :src="item.show ? item.src : ''" alt="">
    <span class="shipment-info">{{item.show ? item.text : ''}}</span>
  </div>
</template>
<script>
export default {
  props:['item']
}
</script>