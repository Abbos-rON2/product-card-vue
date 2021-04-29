<template>
<div class="card-wrapper">
  <div class='card'>
    <div class="card-gallery">
                <hooper group="group1" class="gallery-pagination" ref="carousel" @slide="updateCarousel" :settings="hooperSettings">
              <slide v-for="(item, indx) in product.img" :key="indx" :index="indx">
                <div :class="indx === activeLinkIndex ? 'gallery-item active' : 'gallery-item'" @click="carouselButton(indx)">
                  <img :src='item' alt="img">
                </div>
              </slide>
                <hooper-navigation slot="hooper-addons"></hooper-navigation>
          </hooper>
          <hooper class="gallery-main" group="group1"  :settings="mainHooperSettings" ref="mainCarousel">
              <slide v-for="(item, indx) in product.img" :key="indx" :index="indx">
                <div class="gallery-main-item">
                  <image-zoom :show-message="false" :regular="item"></image-zoom>
                </div>
              </slide>
          </hooper>
    </div>
    <div class="card-info">
      <div class="card-header">{{ product.name }}</div>
      <div class="card-rating">  
        <Rate :rate="product.rate"/>
        <div class="rate-count">{{ product.ratesCount }}</div>
        <div class="ordered">Количество заказов: {{product.ordered}}</div>
      </div>  
      <div class="card-price">{{ num(product.price) }} сум </div>
      <div class="card-sale">
        <div class="old-price">{{ num(product.oldPrice) }} сум </div>
        <div class="discount">-{{ discountPercent }}%</div>
      </div>
      <div class="payment-methods"> 
        <span class="payment-header">Методы оплаты: </span>
        <PaymentItem v-for="(item, index) in product.payment" :key="index" :item="item"/> 
      </div>
      <div class="shipping-methods"> 
        <span class="shipping-header">Методы доставки: </span>
        <ShipmentItem v-for="(item, index) in product.shipment" :key="index" :item="item"/>
         </div>
      <div class="card-quantity">
        <span class="quantity-header">Количество</span>
        <div class="quantity-group">
          <button class="quantity-minus" @click="decrement" :disabled="this.quantity <= 1">-</button>
            <input class="quantity-number" v-model="quantity" type="number" @input="quantityCheck"/>
          <button class="quantity-plus" @click="increment" :disabled="this.quantity >= this.product.inStock">+</button>
        <div class="quantity-inStock"><img :src="check" alt=""> В наличии {{product.inStock}}</div>

        </div>
      </div>
      <div class="btn-group">
        <button class="buy">Купить в один клик</button>
        <button class="contact">Связаться с продавцом</button>
      </div>
      <div class="additional-info">
        <span>* Стоимость доставки заказов до 40 000 сум  - договорная</span>
        <span>** Бесплатная доставка при заказе от 40 000 сум</span>
      </div>
    </div>
  </div>
</div>

</template>

<script>
import Rate from './Info/Rate'
import PaymentItem from './Info/PaymentItem'
import ShipmentItem from './Info/ShipmentItem'

import check from '../assets/check.png'
import arrow from '../assets/arrows.png'

import { Hooper, Slide,  Navigation as HooperNavigation
  } from 'hooper';
import 'hooper/dist/hooper.css';

import '../style.css'

import imageZoom from 'vue-image-zoomer';


export default {
  components: {
    HooperNavigation,
    imageZoom,
    Hooper,
    Slide,
    Rate,
    PaymentItem,
    ShipmentItem
  },
  props: ['product'],
  watch: {
    carouselData () {
      this.$refs.carousel.slideTo(this.carouselData);
    }
  },
  methods:{
    carouselButton(index){
      this.$refs.carousel.slideTo(index);
      this.activeLinkIndex = index
    },
    decrement(){
      if(this.quantity > 1){
      this.quantity--
      }
    },
    increment(){
    if(this.quantity < this.product.inStock){
      this.quantity++
      }
    },
    quantityCheck(){
      if(this.quantity > this.product.inStock){
        this.quantity = this.product.inStock
      } else if(this.quantity <= 0){
        this.quantity = 1;
      }
    },
    scrollTop(){
      this.$refs.carousel.slidePrev();
    },
    scrollBottom(){
      this.$refs.carousel.slideNext();
    },
    changeImg(index){
      this.activeLinkIndex = index;
    },
    updateCarousel(payload) {
      this.myCarouselData = payload.currentSlide;
    },
    num(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
    },
  },
  data(){
    return{
      carouselData: 0,
      quantity: 1,
      check,
      arrow,
      activeLinkIndex: null,
      discountPercent : 100-(this.product.price/(this.product.oldPrice/100)),
      hooperSettings: {
              itemsToShow: 3,
              centerMode: true,
              infiniteScroll: true ,
              breakpoints: {
                1240:{
                  itemsToShow: 7,
                  vertical: true,
                },
                992: {
                  itemsToShow: 9,
                  vertical: false,
                },
                768:{
                  itemsToShow: 7,
                  vertical:false,
                },
                576:{
                  itemsToShow: 5
                },
                360:{
                  itemsToShow: 4
                }
              }
            },
      mainHooperSettings:{
        infiniteScroll: true,
        centerMode: true,
        itemsToShow:1
      }
    }
  },
  mounted() {
    
    setTimeout(() => this.$refs.mainCarousel.updateWidth().bind(this), 50)
    
  },
};
</script>
