<template>
  <div class="main">
    <div>
      <ul v-for="(commande, commandeId) in commandes" :key="commandeId">
        <li>{{ commande }}</li>
        <li>{{ commande.id }} {{ commande.status }} {{ commande.total }}</li>
       
        <ul v-for="commandeItem in commande.line_items">
          <li>{{ commandeItem.product_id}} {{ commandeItem.name}}  {{commandeItem.quantity}}  {{commandeItem.total}}</li>

        </ul>
        <li><commandeIDAdmin  /></li>
      </ul>
    </div>
<button @click="test"></button>
    
  </div>
</template>

<script>
import axios from "axios";
import commandeIDAdmin from '@/components/commandeIDAdmin'

export default {
  name: "affichageCommandesAdmin",
  components:{
    commandeIDAdmin
},
  data() {
    return {
      commandes: [],


      token: localStorage.getItem("token"),
    };
  },
  mounted() {
    this.getCommandesAdmin();
  },
  methods: {
    getCommandesAdmin() {
      axios
        .get("http://applicommande.local/wp-json/wc/v3/orders", {
          headers: { Authorization: "Bearer " + this.token },
        })
        .then((response) => (this.commandes = response.data))
        .catch((e) => e.response)
      
    },
    test() {
        for(let i=0;i < this.commandes.length;i++){
      return  console.log(this.commandes[i].line_items[1])
    }
    }
  },
};
</script>

<style>
</style>