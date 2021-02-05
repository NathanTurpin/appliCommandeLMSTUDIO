<template>
  <div class="main">
    <section id="app" class="content">
      <div v-for="product in products">
          <div class="col">
            <div class="card">
              <img
                v-if="product.images[0] !== undefined"
                :src="product.images[0].thumbnail"
                alt=""
                class="card-img-top"
              />
              <div class="card-body">
                <h5 class="card-title">{{ product.name }}</h5>
                <p class="card-text" v-html="product.description"></p>
                <p class="card-text" v-html="product.price_html"></p>

                <button @click="addToCart(product.id)">{{ product.add_to_cart.text }}</button>
              </div>
            </div>
          </div>
      </div>
    </section>
    <section class="panierTest">
      <div v-for="cart in carts">{{cart}}</div>
      <button @click="order()"></button>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      products: [],
      token : '',
      carts: []
    };
  },
  mounted() {
    this.getProduct();
    axios.post('http://applicommande.local/wp-json/jwt-auth/v1/token', {
        username: 'admin',
        password: 'admin'
      })
      .then(response => (this.token = response.data.token), ( console.log(this.token)))
      .catch(error => console.log(error.response))
  },
  methods: {
    getProduct() {
      axios
        .get("http://applicommande.local/wp-json/wc/store/products")
        .then((response) => (this.products = response.data))
        .catch((error) => console.log(error));
    },
    addToCart(id){
      this.carts.push({
        product_id : id,
        quantity : 1
      })
    },
    order() {
      axios
        .post(
          'http://applicommande.local/wp-json/wc/v3/orders',
          {
            payment_method: 'bacs',
            payment_method_title: 'Direct Bank Transfer',
            set_paid: false,
            billing: {
              first_name: 'john',
              last_name: 'Doe',
              address_1: '969 Market',
              address_2: '',
              city: 'San Francisco',
              state: 'CA',
              postcode: '94103',
              country: 'US',
              email: 'john.doe@example.com',
              phone: '(555) 555-5555'
            },
            shipping: {
              first_name: 'john',
              last_name: 'Doe',
              address_1: '969 Market',
              address_2: '',
              city: 'San Francisco',
              state: 'CA',
              postcode: '94103',
              country: 'US'
            },
            line_items: this.carts,
            shipping_lines: [
              {
                method_id: 'flat_rate',
                method_title: 'Flat Rate',
                total: '10.00'
              }
            ]
          },
          { headers: { Authorization: 'Bearer' + this.token } }
        )
        .then(response => console.log(response.data))
        .catch(error => console.log(error.response))
      this.cart = []
    },
  }
}
</script>

<style>
.col {
    
}
.content {
 display: flex;
  flex-wrap: wrap;
}
.card {
width: 60%;
min-height: 50vh;

}
</style>
