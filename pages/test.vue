<template>
  <div class="main">
    <section id="app" class="content">
      <div v-for="(product, idProduct) in products" :key="idProduct">
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
              <button @click="quantite()">-</button> {{ quantiteProduct }} 
              <button @click="quantite()">+</button>{{ product.id}} 
              <button @click="addPanier(product)">{{ product.add_to_cart.text }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>
    <panier :carts="carts" :products="products" />
    <button @click="teste">test</button>
  </div>
</template>

<script>
import axios from 'axios'
import panier from "@/components/panier"
import CoCartAPI from '@cocart/cocart-rest-api'


export default {
    components: {
    panier,
  },
  data() {
    return {
      products: [],
      product:[],
      carts: [],
      name: '',
      panier:[],
      cartKeys:[],
      quantiteProduct: 1,
    }
  },
 mounted() {

    this.getProduct()
    this.getPanier(); 
    this.test();
 
  
  },
  methods: {
    teste(){
      const querystring = require('querystring');

const store_url = 'https://applicommande.local';
const endpoint = '/wc-auth/v1/authorize';
const params = {
  app_name: 'My App Name',
  scope: 'read_write',
  user_id: 123,
  return_url: 'https://applicommande.local/',
  callback_url: 'https://applicommande.local/callback-endpoint'
};
const query_string = querystring.stringify(params).replace(/%20/g, '+');

console.log(store_url + endpoint + '?' + query_string);
    },
     quantite(){
       this.quantiteProduct++
     },
     wcTest(){
        const api = new WooCommerceRestApi({
        url: "http://applicommande.local",
        consumerKey: "ck_a5f9374d0476952bdec9eca937c8204646fb018e",
        consumerSecret: "cs_e7939dd0fd1012fb2211f09ed94d8d1aab81f780",
        version: "wc/v3"
      });
      api.post("products", donnee)
   .then((response) => {
     console.log(response.data);
   })
  },


    addPanier(product){
      console.log(product.id)
      console.log(this.quantiteProduct)

      const CoCart = new CoCartAPI({
        url: "http://applicommande.local/",
              consumerKey: "ck_a5f9374d0476952bdec9eca937c8204646fb018e",
              consumerSecret: "cs_e7939dd0fd1012fb2211f09ed94d8d1aab81f780",
      });
      var data = {
        "product_id":""+product.id,
        "quantity": this.quantiteProduct
      };


      CoCart.post("add-item", data)
      .then((response) => {
        // Successful request
        console.log("Response Status:", response.status);
        console.log("Response Headers:", response.headers);
        console.log("Response Data:", response.data);
      })
      .catch((error) => {
        // Invalid request, for 4xx and 5xx statuses
        console.log("Response Status:", error.response.status);
        console.log("Response Headers:", error.response.headers);
        console.log("Response Data:", error.response.data);
      })
      .finally(() => {
        // Always executed.
      });

      CoCart.get("get-cart")
      .then((response) => {
        // Successful request
        console.log("Response Status:", response.status);
        console.log("Response Headers:", response.headers);
        console.log("Response Data:", response.data);
      })
      .catch((error) => {
        // Invalid request, for 4xx and 5xx statuses
        console.log("Response Status:", error.response.status);
        console.log("Response Headers:", error.response.headers);
        console.log("Response Data:", error.response.data);
      })
      .finally(() => {
        // Always executed.
      });
    },

    getPanier(){
          const CoCart = new CoCartAPI({
                  url: "http://applicommande.local",
                        consumerKey: "ck_a5f9374d0476952bdec9eca937c8204646fb018e",
                        consumerSecret: "cs_e7939dd0fd1012fb2211f09ed94d8d1aab81f780",
                });
          
          CoCart.get("get-cart")
          .then((response) => {
            // Successful request
            console.log("Response Status:", response.status);
            console.log("Response Headers:", response.headers);
            this.panier=response.data;
            this.cartKeys=Object.keys(response.data);
            console.log("Response CartKey:", this.cartKeys);
            console.log("Response panier:", this.panier);
            console.log(this.cartKeys.length);
            for (let i=0;i<this.cartKeys.length;i++){
                console.log(this.panier[this.cartKeys[i]].product_id);
                this.ajouterAuPanier([this.panier[this.cartKeys[i]].product_id][0],this.panier[this.cartKeys[i]]); 
            }           
          })
          .catch((error) => {
            // Invalid request, for 4xx and 5xx statuses
            console.log("Response Status:", error.response.status);
            console.log("Response Headers:", error.response.headers);
            console.log("Response Data:", error.response.data);
          })
          .finally(() => {
            // Always executed.
          });
    },


   ajouterAuPanier(id,lepanier) {
         axios
        .get('http://applicommande.local/wp-json/wc/store/products/'+id)
        .then(
          response => (
            this.product = response.data,
             console.log(this.product),
            // this.addToCart(this.product,lepanier),
             this.carts.push({
               product: this.product,
               panier: lepanier
             } )
            ),
        )
        .catch(error => console.log(error))
    },
 
    getProduct() {
      axios
        .get('http://applicommande.local/wp-json/wc/store/products')
        .then(
          response => (
            this.products = response.data,
             console.log(this.products)
            ),
        )
        .catch(error => console.log(error))
    },


    test() {
        console.log('list rest api-------------')
      axios.get('http://applicommande.local/wp-json/wc/v3')
        .then(
          response => (console.log (response.data))
        )
        .catch(error => console.log(error))
    }
  },

 
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
