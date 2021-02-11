<template>
  <div class="main">
<button @click="test">cc</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
data(){
    return{
        users: []
    }
},
   mounted(){
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
    test(){
        axios.get("http://applicommande.local/wp-json/wp/v2/users")
       .then(response => console.log(response.data))
    .catch(error => console.log(error.response))
    }
}
}
</script>

<style>

</style>