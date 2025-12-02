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
      <button v-on:click="addBurger">+</button>
      <button v-on:click="deleteBurger" :disabled="amountOrdered === 0">
        -
      </button>
      <!--fick hjälp från ChatGPT med :disabled="amountOrdered === 0"-->
      <p>Amount wanted: {{ amountOrdered }}</p>
    </div>
  </div>
</template>

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
  methods: {
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
    },

    deleteBurger: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered -= 1;
        this.$emit("orderedBurger", {
          name: this.burger.name,
          amount: this.amountOrdered,
        });
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*Justerar dom små burgarboxarna*/
.box {
  width: px;
  background-color: crimson;
  color: #eec2c2;
  border-radius: 30px;
  padding: 20px;
  font-size: 100%;
  font-weight: bold;
}
</style>
