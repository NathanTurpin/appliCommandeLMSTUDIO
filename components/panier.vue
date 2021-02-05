<template>
  <div class="panier">
    <b-button v-b-modal.modal-scrollable>Panier</b-button>

    <b-modal id="modal-scrollable" scrollable title="BootstrapVue" hide-footer>
      <div v-for="product in products" class="my-4">
        <div class="containerCart" >
          <div class="rowCart">
            <img
              v-if="product.images[0] !== undefined"
              :src="product.images[0].thumbnail"
              alt=""
              class="cart-img"
            />

            {{ product.name }}
            <!-- {{ cart.quantity }} -->
           
          </div>
            <div class="rowCartPrice" v-html="product.price_html"></div>

        </div>
      </div>
      <b-button class="mt-3" block @click="test">Payer</b-button>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: "panier",
  props: ["carts", "products"],
  data() {
    return {};
  },
  methods: {
    test() {
      alert("cc");
      this.$emit('payer-panier', "payer")
    },
  },
};
</script>

<style scoped>
img {
  width: 20%;
}
.containerCart {
  display: flex;
  flex-wrap: wrap;
}
.rowCart {
flex: 3 1 0;
}
.rowCartPrice {
  flex: 1 1 0;
}
</style>