<template>
  <div class='card'>
    <div class="card-gallery">
        <button class="scroll-top" @click="scrollTop">
          <img :src="arrow" alt="">
        </button>
        <button class="scroll-bottom" @click="scrollBottom">
          <img :src="arrow" alt="">
        </button>
        <div class="gallery-pagination">
        <GalleryItem 
          v-for="(item, index) in product.img" 
          :key="index" 
          :index="index" 
          :active="index === activeLinkIndex"
          :path="item" 
          @change-img="changeImg"/>
        </div>
        <div class="gallery-main">
          <img :src="this.product.mainImg" alt="" :key="this.product.mainImg">
        </div>        
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
import GalleryItem from './GalleryItem'
import Rate from './Rate'
import PaymentItem from './PaymentItem'
import ShipmentItem from './ShipmentItem'

import check from '../assets/check.png'
import arrow from '../assets/arrows.png'

export default {
  props: ['product'],
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
      } else if(this.quantity < 0){
        this.quantity = 0;
      }
    },
    scrollTop(){
      const div = document.querySelector('.gallery-pagination');
      if(div.scrollTop){
        div.scroll({
          top: div.scrollTop -100,
          behavior: 'smooth'
        })
        }
    },
    scrollBottom(){
      const div = document.querySelector('.gallery-pagination');
        div.scroll({
          top: div.scrollTop +100,
          behavior: 'smooth'
        })
    },
    changeImg([path, index]){
      this.$emit('change-img', path);
      this.activeLinkIndex = index;
    },
    num(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
}
  },
  components: {
    GalleryItem,
    Rate,
    PaymentItem,
    ShipmentItem
  },
  data(){
    return{
      quantity: 1,
      check,
      arrow,
      activeLinkIndex: null,
      discountPercent : 100-(this.product.price/(this.product.oldPrice/100)),
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Rubik:wght@400;500&display=swap');
*{
  font-family: "Rubik";
}

.additional-info{
  font-size: 15px;
  line-height: 140%;
  color: #596178;
}
.additional-info span{
  display: block;
  margin-top: 12px;
}
.scroll-top,
.scroll-bottom{
  width: 37px;
  height: 36px;
  z-index: 10;
  padding: 6px;
  position: absolute;
  background: #FFFFFF;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
  filter: drop-shadow(0px 2px 5px rgba(40, 41, 61, 0.1));
}
.payment-methods,
.shipping-methods{
  font-size: 13px;
  line-height: 140%;
  color: #596178;
}
.shipping-methods{
  border-bottom: 1px solid #E1DFDF;
  display: flex;
  align-items: center;
  padding-bottom: 20px;
  margin-bottom: 15px;
}
.shipping-header{
  margin-right: 28px;
}
.shipment-item{
  color: black;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-right: 20px;
}
.shipment-img{
  margin-right: 4px;
}
.payment-item{
  margin-right: 16px;
}
.payment-header{
margin-right: 16px;
}
.payment-methods{
  display: flex;
  align-items: center;
  margin-bottom: 19px;
}
.payment-item{
  display: flex;
  justify-content: center;
  align-self: center;
}
.scroll-top{
  left: 14px;
  top: 4px;
}
.scroll-bottom{
  bottom: 18px;
  left: 13px;
}
.scroll-bottom img{
  transform: rotate(180deg);
}

button{
  outline: none;
  cursor: pointer;
}
.rate{
  display: flex;
}
.rate-star{
  margin-right: 8px;
}
.rate-count,
.ordered{
  font-size: 11px;
  line-height: 140%;
  color: #596178;
}
.ordered{
  padding-bottom: 4px;
  margin-left: 48px;
}
.card-rating{
  padding-bottom: 17px;
  margin-left: 8px;
  margin-bottom: 24px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #E1DFDF;
}
.card-quantity{
  display: flex;
  flex-direction: column;
}
.quantity-header{
  font-size: 13px;
  line-height: 15px;
  color: #2F323D;
}
.quantity-inStock{
  margin-left: 20px;
  height: fit-content;
  padding: 0 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #EFEEB4;
  border-radius: 4px;
  font-size: 11px;
  color: #596178;
}
.quantity-group{
  margin: 14px 0 33px;
  display: flex;
  align-items: center;
}
.quantity-number{
  align-self: stretch;
  outline: none;
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
  width: 60px;
  margin: 0 8px;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
  font-size: 17px;
  line-height: 140%;
  background-color: #F6F6F6;
  color: #2F323D;
}
.quantity-number::-webkit-outer-spin-button,
.quantity-number::-webkit-inner-spin-button {
    -webkit-appearance: none;
}
.quantity-minus,
.quantity-plus{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 42px;
  height: 42px;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
  background-color: #fff;
  color: #ccc;
  font-size: 24px;
}
.quantity-minus:hover,
.quantity-plus:hover{
  transition: .3s;
  color: white;
  background-color: #309975;
}
.quantity-minus:active,
.quantity-plus:active {
    transform: scale(1.2);
}
.quantity-minus:disabled,
.quantity-plus:disabled{
    background-color: #F6F6F6;
    cursor: not-allowed;
}


.card-gallery{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-right: 46px;
  position: relative;
}
.gallery-pagination{
  margin-top: 13px;
  position: relative;
  overflow: scroll;
  margin-right: 4px;
  width: 65px;
  display: flex;
  flex-direction: column;
  margin-bottom: 30px;
}
/* .gallery-pagination::after{
  position: absolute;
  bottom: 0;
  top: 0;
  left: 0;
  right: 0;
  content: '';
  z-index: 3;
  -webkit-box-shadow: inset 0px 50px 50px -14px #FFFFFF, inset 0px -50px 50px -14px #FFFFFF; 
  box-shadow: inset 0px 50px 50px -14px #FFFFFF, inset 0px -50px 50px -14px #FFFFFF;
} */
.gallery-pagination::-webkit-scrollbar {
  display: none;
}
.gallery-item{
  cursor: pointer;
  margin-bottom: 12px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 65px;
  height: 65px;
  background: #FFFFFF;
  border: 1px solid #E0E0E0;
  box-sizing: border-box;
  border-radius: 4px;
}
.gallery-item.active{
  transform: scale(1.02);
  transition: .3s;
  border: 1px solid #309975;
}
.gallery-main{
  margin-top: 9px;
  border: 1px solid #E0E0E0;
  box-sizing: border-box;
  border-radius: 8px;
  width: 500px;
  height: 550px;
  display: flex;
  justify-content: center;
  align-items: center;
}


.card{
  background-color: #fff;
  width: 1240px;
  height: 664px;
  box-shadow: 0px 0px 1px rgba(40, 41, 61, 0.04), 0px 2px 4px rgba(96, 97, 112, 0.16);
  border-radius: 8px;
  display: flex;
  padding: 50px 75px;
  padding-left: 20px;
}
.card > div{
  width: 50%;
}


.card-header{
  margin-bottom: 7px;
  font-style: normal;
  font-weight: 500;
  font-size: 20px;
  line-height: 24px;
  color: #2F323D;
}
.card-price{
  font-style: normal;
  font-weight: 500;
  font-size: 24px;
  line-height: 125%;
  color: #2F323D;
  margin-bottom: 8px;
}
.card-sale{
  display: flex;
  align-items: center;
  margin-bottom: 14px;
}
.old-price{
  margin-left: 1px;
  font-size: 13px;
  line-height: 15px;
  color: #596178;
  text-decoration: line-through;
}
.discount{
  margin-top: 2px;
  margin-left: 23px;
  transition: .3s;
  color: white;
  padding: 2px 5px 3px;
  background: #FF5530;
  border-radius: 2px;
  font-size: 13px;
  line-height: 140%;
}
.btn-group{
  border-bottom: 1px solid #E1DFDF;
}
.btn-group button{
  transition: .3s;
  font-size: 17px;
  font-style: normal;
  font-weight: 400;
  line-height: 24px;
  letter-spacing: 0em;
  border: 0;
  border-radius: 4px;
  display: inline-block;
  padding: 8px 32px;
  margin-bottom: 24px;
}
button.buy{
  margin-right: 16px;
  background: #309975;
  color: white;
    border: 1px solid #309975;
}
button.buy:hover{
  background-color: #fff;
  color: #309975;
  border: 1px solid #E0E0E0;
}
button.contact{
  background-color: #fff;
  color: #2F323D;
  border: 1px solid #E0E0E0;
}
button.contact:hover{
  background-color: #309975;
  color: #fff;
  border: 1px solid #E0E0E0;
}
</style>
