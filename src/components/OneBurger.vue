<template>
  <div class="box">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.url" style="width: 100%" />
    <ul>
      <li v-if="burger.gluten == true">Contains Gluten</li>
      <li v-else>Gluten-free</li>
      <li v-if="burger.lactose == true">Contains Lactose</li>
      <li v-else>Lactose-free</li>
      <li>{{ burger.protein }}</li>
    </ul>
    <div class="amountbuttons">
      <button v-on:click="amountOrdered++">+</button>
      <button v-on:click="amountOrdered--" :disabled="amountOrdered === 0">
        <!--fick hjälp från ChatGPT med :disabled="amountOrdered === 0"-->
        -
      </button>

      <p>Amount ordered: {{ amountOrdered }}</p>
    </div>
  </div>

  <!-- Exempel från HomeView
      <section id="thesubmitbutton">
        <button type="submit" v-on:click="submitOrder">Submit order!</button>
      </section>
         <section id="increasebutton">
        <button v-on:click="amountOrdered++">Add burger</button>
      </section>
--></template>

<script>
export default {
  name: "OneBurger",
  props: {
    burger: Object,
  },
  data: function () {
    return {
      amountOrdered: 0,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*Justerar burgarboxarna*/
.box {
  width: px;
  background-color: crimson;
  color: black;
  border-radius: 30px;
  padding: 20px;
  font-size: 100%;
  font-weight: bold;
}
</style>
