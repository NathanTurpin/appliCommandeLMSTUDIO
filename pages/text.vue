<template>
  <div class="containerArticle">
    <p>
      titre: <input type="text" v-model="actu.title" /> contenu :
      <input type="text" v-model="actu.content" />
      <input type="text" v-model="actu.status" />
      <a href="#" @click="envoyer()" class="btn btn-success">Envoyer</a>
    </p>
    <h2>produit commandé</h2>
    <p v-for="cart in carts">
      {{ cart }}
    </p>
    nom :<input type="text" v-model="user" />
    <button @click="order()"></button>
    <div v-for="(product, index) in products" :key="index">
      <div class="card ">
        <img
          v-if="product.images[0] !== undefined"
          :src="product.images[0].thumbnail"
          alt=""
          class="card-img-top"
        />
        <div class="card-body">
          <h5 class="card-title">{{ product.name }}</h5>
          <p class="card-text" v-html="product.short_description"></p>

          <p v-html="product.price_html"></p>
          <small class="text-muted" v-if="product.is_in_stock">En stock</small>
          <div class="card-footer">
            <!-- <a :href="product.add_to_cart.url">{{
              product.add_to_cart.text
            }}</a> -->
            <button @click="addProduct(product.id)">Commander</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      products: [],
      carts: [],
      name: '',
      token: '',
      actu: {
        title: '',
        content: '',
        status: ''
      }
    }
  },
  methods: {
    getProduct() {
      axios
        .get('http://applicommande.local/wp-json/wc/store/products')
        .then(
          response => (this.products = response.data),
          console.log(this.products)
        )
        .catch(error => console.log(error))
    },
    addProduct(id) {
      this.carts.push({
        product_id: id,
        quantity: 1
      }),
        console.log('ok')
    },
    order() {
      axios
        .post(
          'http://applicommande.local/wp-json/wc/v3/orders',
          {
            payment_method: 'bacs',
            payment_method_title: 'Direct Bank Transfer',
            set_paid: true,
            billing: {
              first_name: this.user,
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
              first_name: this.user,
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
    envoyer() {
      axios
        .post(
          'http://applicommande.local/wp-json/wp/v2/posts',
          {
            title: this.actu.title,
            content: this.actu.content,
            status: this.actu.status
          },
          { headers: { Authorization: 'Bearer' + this.token } }
        )
        .then(response => console.log(response.data))
        .catch(error => console.log(error.response))
    }
  },
  mounted() {
    this.getProduct()
    //RECUPERE LE TOKEN
    axios
      .post('http://applicommande.local/wp-json/jwt-auth/v1/token', {
        username: 'admin',
        password: 'admin'
      })
      .then(response => (this.token = response.data.token), ( console.log(this.token)))
      .catch(error => console.log(error.response))
      
  }
 
}
</script>

<style scoped>
.containerArticle {
  display: flex;
  flex-wrap: wrap;
  width: 80%;
  margin: auto;
}
.card {
  margin: 1em 2em;
  width: 80%;
}
</style>
