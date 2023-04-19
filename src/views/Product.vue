<template>
  <div>
    <div class="nav-bar"></div>
    <div class="cart">Cart({{ cart.length }})</div>
    <ProductDisplay :premium="premium"></ProductDisplay>
    <ProductDisplay :premium="false"></ProductDisplay>
    <ProductDisplay :premium="false"></ProductDisplay>
  </div>
</template>
<script setup>
import ProductDisplay from '@/components/ProductDisplay.vue'
import { reactive, ref, computed } from 'vue'
const cart = ref([])
const premium = ref(true)
</script>
<style scoped lang="scss">
.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 25px;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.57);
}
.cart {
  margin: 25px 100px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 10px 30px;
  background-color: white;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}
</style>
