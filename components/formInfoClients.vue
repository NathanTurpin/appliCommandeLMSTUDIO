<template>
  <div class="main">
    <h2>Facturation :</h2>
    <b-form @reset="onReset" v-if="show">
      <b-form-group
        id="input-group-2"
        label="Votre prénom :"
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          v-model="formBilling.first_name"
          placeholder="John"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Votre nom :" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="formBilling.last_name"
          placeholder="Doe"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Adresse 1 :" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="formBilling.address_1"
          placeholder=""
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Ville :" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="formBilling.city"
          placeholder="Doe"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Pays :" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="formBilling.country"
          placeholder="Doe"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-2"
        label="Code postal :"
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          v-model="formBilling.postcode"
          placeholder="Doe"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
        description="We'll never share your email with anyone else."
      >
        <b-form-input
          id="input-1"
          v-model="formBilling.email"
          type="email"
          placeholder="Enter email"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Tél :" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="formBilling.phone"
          placeholder="Doe"
          required
        ></b-form-input>
      </b-form-group>
      <b-button class="mt-3" @click="validation()">validation</b-button>

      <b-button class="mt-3" @click="order()">Commander</b-button>

      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "formInfoClients",
  data() {
    return {
      formBilling: {
        first_name: "John",
        last_name: "Doe",
        address_1: "969 Market",
        address_2: "",
        city: "San Francisco",
        state: "CA",
        postcode: "94103",
        country: "US",
        email: "john.doe@example.com",
        phone: "(555) 555-5555",
      },
      carts: [],
      show: true,
      token: "",
      finalBilling: [],
      finalCarts: [],
    };
  },
  mounted() {
    // TOKEN
    axios
      .post("http://applicommande.local/wp-json/jwt-auth/v1/token", {
        username: "admin",
        password: "admin",
      })
      .then(
        (response) => (this.token = response.data.token),
        console.log(this.token)
      )
      .catch((error) => console.log(error.response));
  },
  methods: {
    //SAVE INFO FACTURATION DS LOCAL STORAGE
    saveFormBilling() {
      let parsed = JSON.stringify(this.formBilling);
      localStorage.setItem("formBilling", parsed);
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.formBilling.email = "";
      this.formBilling.name = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    validation() {
      this.saveFormBilling();
      this.formBilling = localStorage.getItem("formBilling");
      this.finalBilling = JSON.parse(this.formBilling);
      this.carts = localStorage.getItem("cartsIDQuant");
      this.finalCarts[0] = JSON.parse(this.carts);
    },
    // CREER UNE COMMANDE
    order() {
      console.log(this.finalCarts);
      axios
        .post(
          "http://applicommande.local/wp-json/wc/v3/orders",
          {
            payment_method: "bacs",
            payment_method_title: "Direct Bank Transfer",
            set_paid: false,
            billing: {
              first_name: this.finalBilling.first_name,
              last_name: this.finalBilling.last_name,
              address_1: this.finalBilling.address_1,
              address_2: "",
              city: this.finalBilling.city,
              state: "",
              postcode: this.finalBilling.postcode,
              country: this.finalBilling.country,
              email: this.finalBilling.email,
              phone: this.finalBilling.phone,
            },
            shipping: {
              first_name: "john",
              last_name: "Doe",
              address_1: "969 Market",
              address_2: "",
              city: "San Francisco",
              state: "CA",
              postcode: "94103",
              country: "US",
            },
            line_items: this.finalCarts[0],
            shipping_lines: [
              {
                method_id: "flat_rate",
                method_title: "Flat Rate",
                total: "10.00",
              },
            ],
          },
          { headers: { Authorization: "Bearer " + this.token } }
        )
        .then((response) => console.log(response.data))
        .catch((error) => console.log(error.response));
    },
  },
};
</script>