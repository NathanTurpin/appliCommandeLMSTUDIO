<template>
  <div class="main">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
        description="We'll never share your email with anyone else."
      >
        <b-form-input
          id="input-1"
          v-model="form.email"
          type="email"
          placeholder="Enter email"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.name"
          placeholder="Enter name"
          required
        ></b-form-input>
      </b-form-group>

     

      <b-button type="button" @click="onSubmit()" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
    
  </div>
</template>

<script>
import axios from 'axios'
  export default {
      name: 'formClient',
    data() {
      return {
        form: {
          email: '',
          name: '',
        },
        show: true,
      token: "",
   
      }
    },mounted() {
   
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
      onSubmit() {
        // event.preventDefault()
        axios.post('http://applicommande.local/wp-json/wc/v3/customers', 
        {
            email: this.form.email,
            first_name: this.form.name,
            // last_name: 'this.form.last_name',
            // username: 'this.form.username',
            // billing: {
            //     first_name: 'this.form.first_name',
            //     last_name: 'this.form.last_name',
            //     company: 'this.form.company',
            //     address_1: 'this.form.address_1',
            //     address_2: this.form.address_2,
            //     city: this.form.city,
            //     state: this.form.state,
            //     postcode: this.form.postcode,
            //     country: this.form.country,
            //     email: this.form.email,
            //     phone: this.form.phone
            // },
            // shipping: {
            //     first_name: this.form.first_name,
            //     last_name: this.form.last_name,
            //     company: this.form.company,
            //     address_1: this.form.address_1,
            //     address_2: this.form.address_2,
            //     city: this.form.city,
            //     state: this.form.state,
            //     postcode: this.form.postcode,
            //     country: this.form.country,
            // }
            }, { headers: { Authorization: "Bearer" + this.token } }
            )
        .then((response) => console.log(response.data))
        .catch((error) => console.log(error.response));
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }
</script>