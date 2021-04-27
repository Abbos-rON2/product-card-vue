<template>
  <div class='card'>
    <div class="card-gallery">
        <button class="scroll-top" @click="scrollTop">
          <img :src="arrow" alt="">
        </button>
        <button  class="scroll-bottom" @click="scrollBottom">
          <img :src="arrow" alt="">
        </button>
          <hooper 
          :itemsToSlide="1"
          ref="carousel" @slide="updateCarousel"
          class="gallery-pagination" 
          :itemsToShow="7" group="group1" 
          :centerMode="true" 
          :infiniteScroll="true" 
          :vertical="true">
              <slide v-for="(item, indx) in product.img" :key="indx" :index="indx">
                <div :class="indx === activeLinkIndex ? 'gallery-item active' : 'gallery-item'">
                  <img :src='item' alt="img">
                </div>
              </slide>
          </hooper>
          <hooper class="gallery-main" group="group1">
              <slide v-for="(item, indx) in product.img" :key="indx" :index="indx">
                <div class="gallery-main-item">
                  <image-zoom :show-message="false"
                    :regular="item">				
                  </image-zoom>
                  <!-- <img :src='item' alt="img"> -->
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
</template>

<script>
import Rate from './Info/Rate'
import PaymentItem from './Info/PaymentItem'
import ShipmentItem from './Info/ShipmentItem'

import check from '../assets/check.png'
import arrow from '../assets/arrows.png'

import { Hooper, Slide} from 'hooper';
import 'hooper/dist/hooper.css';

import '../style.css'

import imageZoom from 'vue-image-zoomer';


export default {
  components: {
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
}
  },
  data(){
    return{
      carouselData: 0,
      quantity: 1,
      check,
      arrow,
      activeLinkIndex: null,
      discountPercent : 100-(this.product.price/(this.product.oldPrice/100)),

    }
  }
};
</script>
