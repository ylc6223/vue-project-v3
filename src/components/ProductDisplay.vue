<template>
  <div class="product-display">
    <div class="product-container">
      <div class="product-image">
        <a :href="url">
          <img :class="{ 'out-of-stock-img': inStock <= 0 }" :src="image" />
        </a>
      </div>
      <div class="product-info">
        <h1 class="product-title">{{ title }}</h1>
        <p>{{ desc }}</p>
        <p v-if="inStock > 10">剩余库存{{ inStock }}</p>
        <p v-else-if="inStock <= 10 && inStock > 0">即将售罄!库存：{{ inStock }}!</p>
        <p v-else>售罄</p>
        <div class="flex">
          <ul class="flex-1">
            <li v-for="(detail, index) in details" :key="index">{{ detail }}</li>
          </ul>
          <p class="flex-1">运费: {{ shipping }}</p>
        </div>

        <div v-for="(variant, index) in variants" :key="variant.id">
          <!--          <div class="flex">-->
          <!--            <label :for="index">-->
          <!--              <span-->
          <!--                class="flex-1 text-xl bold"-->
          <!--                :class="{ active: variant.image === selectedVariant.image }"-->
          <!--              >-->
          <!--                {{ variant.color }}-->
          <!--              </span>-->

          <!--              <span-->
          <!--                v-if="false"-->
          <!--                class="color-circle"-->
          <!--                :style="{-->
          <!--                  backgroundColor: variant.image === selectedVariant.image ? variant.color : '#FFF'-->
          <!--                }"-->
          <!--              ></span>-->
          <!--            </label>-->
          <!--            <input-->
          <!--              :id="index"-->
          <!--              type="radio"-->
          <!--              name="color"-->
          <!--              :value="variant"-->
          <!--              v-model="selectedVariant"-->
          <!--              :checked="index == 0"-->
          <!--            />-->
          <!--          </div>-->
          <input
            type="radio"
            :name="radioName"
            :value="variant"
            v-model="selectedVariant"
            :checked="index === 0"
          />
        </div>

        <div class="flex items-center">
          <div>尺码:</div>
          <div v-for="(size, index) in sizes" :key="index">{{ size }}</div>
        </div>

        <button class="button" :class="{ disabledButton: inStock <= 0 }" @click.prevent="add2Cart">
          Add to Cart
        </button>
        <button class="button" @click="removeFromCart">Remove Item</button>
      </div>
    </div>
  </div>
</template>
<!--<script setup> 是在单文件组件 (SFC) 中使用组合式 API 的编译时语法糖-->
<script setup lang="ts">
import { reactive, ref, computed } from 'vue'
//defineProps 是一个仅 <script setup> 中可用的编译宏命令，并不需要显式地导入
interface Props {
  premium: boolean //是否为付费用户
  radioName: string
}
const props = withDefaults(defineProps<Props>(), {
  premium: false
})

const product = ref('冬款长袜')
const brand = ref('Vue Mastery')
const desc = ref('一双暖暖的毛绒袜子')
const variants = reactive([
  {
    id: 2234,
    color: 'green',
    image: 'src/assets/images/socks_green.jpg',
    quantity: 50
  },
  {
    id: 2235,
    color: 'blue',
    image: 'src/assets/images/socks_blue.jpg',
    quantity: 0
  }
])
const url = ref(
  'https://images.pexels.com/photos/160438/ankle-socks-children-socks-knitted-greeting-160438.jpeg'
)
const details = reactive(['50% 棉花', '30% 羊毛', '20% 聚酯纤维'])
const sizes = reactive(['S', 'M', 'L', 'XL'])
const selectedVariant = ref(variants[0])

const title = computed(() => {
  return brand.value + ' ' + product.value
})
const shipping = computed(() => {
  if (props.premium) {
    return '免邮费'
  }
  return '2.99$'
})

//要显示的图片
const image = computed({
  get() {
    return selectedVariant.value.image
  },
  set(newValue) {
    selectedVariant.value.image = newValue
  }
})
//库存
const inStock = computed(() => {
  return selectedVariant.value.quantity
})
//添加到购物车
function add2Cart() {
  $emit()
  // if (cart.value.length >= inStock.value) {
  //   alert('购买数量不能超过剩余库存')
  //   return
  // }
  // cart.value.push({
  //   productName: 'green socks',
  //   size: 'M'
  // })
}
//更换商品图片
function updateVariant(variant, e) {
  // console.log('change img')
  // console.log(e.target.value)
  // //重置上次勾选
  // selectedVariant.value.checked = false
  // //将本次点击的进行勾选
  // variant.checked = e.target.checked
  // //修改图片
  // image.value = e.target.value
  //
  // selectedVariant.value = variant
}
</script>

<style scoped lang="scss">
* {
  margin: 0;
  padding: 0;
}
.flex {
  display: flex;
}
.justify-center {
  justify-content: center;
}
.items-center {
  align-items: center;
}
.flex-1 {
  flex: 1;
}
.text-xl {
  font-size: 20px;
}
.bold {
  font-weight: bold;
}
.active {
  color: #0075ff;
}

body {
  background-color: #f2f2f2;
  margin: 0px;
  font-family: tahoma;
  color: #282828;
}

.button {
  margin: 30px;
  background-color: #39495c;
  border-radius: 5px;
  font-size: 18px;
  width: 160px;
  height: 60px;
  color: white;
  padding: 20px;
  box-shadow: inset 0 -0.6em 1em -0.35em rgba(0, 0, 0, 0.17),
    inset 0 0.6em 2em -0.3em rgba(255, 255, 255, 0.15),
    inset 0 0 0em 0.05em rgba(255, 255, 255, 0.12);
  text-align: center;
  cursor: pointer;
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

.color-circle {
  width: 50px;
  height: 50px;
  margin-top: 8px;
  border: 2px solid #d8d8d8;
  border-radius: 50%;
}

.disabledButton {
  background-color: #d8d8d8;
  cursor: not-allowed;
}

h1 {
  font-size: 40px;
}

h3 {
  font-size: 25px;
}

img {
  border: 2px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  padding: 15px;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}

input {
  width: 100%;
  height: 40px;
  //margin-bottom: 20px;
}

label {
  font-size: 20px;
  margin-bottom: 5px;
}

li {
  font-size: 18px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 25px;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.57);
}

.out-of-stock-img {
  opacity: 0.5;
}

p {
  font-size: 22px;
}

.product-display {
  display: flex;
  flex-direction: column;
  padding: 1rem;
}

.product-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.product-image,
.product-info {
  width: 50%;
}
.product-title {
  color: var(--vt-title-purple);
}
.review-form {
  display: flex;
  flex-direction: column;
  width: 425px;
  padding: 20px;
  margin: 40px;
  border: 2px solid #d8d8d8;
  background-color: white;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}

.review-container {
  width: 425px;
  padding: 20px;
  background-color: white;
  -webkit-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 20px -12px rgba(0, 0, 0, 0.57);
  margin-left: 40px;
  border: 2px solid #d8d8d8;
}

.review-container li {
  margin-bottom: 30px;
}

.review-form .button {
  display: block;
  margin: 30px auto;
}

select {
  height: 40px;
  font-size: 20px;
  background-color: white;
  cursor: pointer;
}

textarea {
  width: 95%;
  height: 70px;
  padding: 10px;
  font-size: 20px;
  margin-bottom: 20px;
}

ul {
  list-style-type: none;
}

@media only screen and (max-width: 600px) {
  .container {
    flex-direction: column;
  }

  .product-image,
  .product-info {
    margin-left: 10px;
    width: 100%;
  }

  .review-form {
    width: 90%;
  }
}
</style>
