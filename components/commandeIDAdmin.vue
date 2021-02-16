<template>
<div>
  <div v-for="command in commande">
      {{command}}
      <button></button>
  </div>
  </div>
</template>

<script>
export default {
    props:["commande"]

}
</script>

<style>

</style>