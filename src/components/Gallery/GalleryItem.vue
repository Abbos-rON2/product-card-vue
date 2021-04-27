<template>
<div :class="active ? 'gallery-item active' : 'gallery-item'" @click="changeImg">
  <img :src='path' alt="img">
</div>
</template>

<script>
export default {
  props: {
    path:{
      type: String,
      required: true,
    },
    active:{
      type: Boolean,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    }
  },
  methods: {
    changeImg(){
      this.$emit('change-img',  [this.path, this.index])
    }
  }, 
}
</script>