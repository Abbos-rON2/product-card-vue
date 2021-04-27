<template>
  <div id="app">
    <Card v-bind:product="product" @change-img="changeImg"/>
  </div>
</template>

<script>
import Card from "./components/Card";

import img1 from './assets/1.png';
import img2 from './assets/2.png';
import img3 from './assets/3.png';
import img4 from './assets/4.png';
import img5 from './assets/5.png';

import payme from './assets/Payme.png'
import click from './assets/Click.png'
import humo from './assets/Humo.png'

import delivery from './assets/delivery.png'
import selfDelivery from './assets/ad-product.png'
export default {
  name: "App",
  data(){
    return {
      product:{
        name: '100% органический 10 лет корень корейского женьшеня красный Panax улучшить иммунную',
        price: 10000000,
        oldPrice: 20000000,
        rate: [1, 1, 1,1, 1],
        ratesCount: 243, 
        inStock: 1245,
        ordered: 1,
        payment: {
          payme: {src: payme, show: true},
          humo: {src: humo, show : true}, 
          click: {src: click, show: true},
         },
        shipment: {
          deliver: {src: delivery, text: 'Доставка продавца', show: true},
          selfDeliver: {src: selfDelivery, text: 'Самовывоз', show: true}
        },
        img: [img1, img2, img3, img4, img5]
      }
    }
  },
  components: {
    Card
  },
  methods:{
    changeImg(path){
      this.product.mainImg = path;
    }
  }
};
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background: #E5E5E5;
}
</style>
