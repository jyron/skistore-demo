<template>
  <div class="container mt-4">
  <input type='text' v-model='city' placeholder = 'Enter a city'/>
     
     <h3>{{weatherByCity}}</h3>

    <div class="row">
      <div class="col-md-7">
        <div class="row">
          <div class="col-md-6" :key="product.id" v-for="product in findproduct">
            <product
              :isInCart="isInCart(product)"
              v-on:add-to-cart="addToCart(product)"
              :product="product"
            ></product>
          </div>
        </div>
      </div>
      <div class="col-md-5 my-5">
        <cart v-on:pay="pay()" v-on:remove-from-cart="removeFromCart($event)" :items="cart"></cart>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import products from "@/products.json";
import Product from "@/components/Product.vue";
import Cart from "@/components/Cart.vue";

export default {
  name: "app",
  components: {
    Product,
    Cart
  },
  data() {
    return {
      products,
      cart: [],
      selected: '',
      info: 'hey',
      city:''
    };
  },
  computed: {
    weatherByCity: function() {
      if (this.info < 30) {
        return this.selected = 'cold';
      } else if (this.info < 60) {
        return this.selected = 'rainy';
      } else if (this.info > 60) {
        return this.selected = 'hot';
      }
    },

    findproduct: function () {
      if (this.selected == '') {
        return this.products;
      }
    return this.products.filter((product) => {
      return product.weather === this.selected;
    })
  }
},
watch: {
  city: function(val)  {
  axios
  .get(`https://api.openweathermap.org/data/2.5/weather?q=${val}&units=imperial&appid=148bce663ab035793645b419cd596823`)
  .then(response => (this.info = response.data.main.temp))
  }
},
  methods: {
    addToCart(product) {
      this.cart.push(product);
    },
    isInCart(product) {
      const item = this.cart.find(item => item.id === product.id);
      if (item) {
        return true;
      }
      return false;
    },
    removeFromCart(product) {
      this.cart = this.cart.filter(item => item.id !== product.id);
    },
    pay() {
      this.cart = [];
      alert("Thanks! Shopping successfully completed. ");
    },

  }
};
</script>

<style>
body {
  background-color: #dcdcdc;
}
</style>


