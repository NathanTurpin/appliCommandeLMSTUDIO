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

                <button>{{ product.add_to_cart.text }}</button>
              </div>
            </div>
          </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      products: [],
      token : ''
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
  },
};
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
