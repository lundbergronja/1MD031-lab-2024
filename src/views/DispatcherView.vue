<template>
  <div id="orders">
    <div id="orderList">
      <div v-for="(order, key) in orders" v-bind:key="'order' + key">
        <strong> #{{ key }}:</strong>
        <div class="rubrik">Order:</div>
        <div v-for="(amount, name) in order.orderItems" :key="name">
          {{ name }}: {{ amount }}
        </div>
        <div class="rubrik">Customer info:</div>
        <div class="customer">
          <div>Name: {{ order.customerInfo.customerName }}</div>
          <div>Gender: {{ order.customerInfo.gender }}</div>
          <div>Payment method: {{ order.customerInfo.paymentMethod }}</div>
        </div>
        <hr />
      </div>
      <button v-on:click="clearQueue">Clear Queue</button>
    </div>
    <div id="dots">
      <div
        class="target"
        v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }"
      >
        T
      </div>

      <div
        v-for="(order, key) in orders"
        v-bind:style="{
          left: order.details.x + 'px',
          top: order.details.y + 'px',
        }"
        v-bind:key="'dots' + key"
      >
        {{ "T" }}
      </div>
    </div>
  </div>
</template>
<script>
import io from "socket.io-client";
const socket = io("localhost:3000");

export default {
  name: "DispatcherView",
  data: function () {
    return {
      orders: {},
      location: { x: 0, y: 0 },
    };
  },
  created: function () {
    socket.on("currentQueue", (data) => (this.orders = data.orders));
  },
  methods: {
    clearQueue: function () {
      socket.emit("clearQueue");
    },
    changeStatus: function (orderId) {
      socket.emit("changeStatus", { orderId: orderId, status: "Annan status" });
    },
  },
};
</script>
<style>
#orderList {
  top: 1em;
  left: 1em;
  position: absolute;
  z-index: 2;
  color: black;
  background: rgba(255, 255, 255, 0.5);
  padding: 1em;
}

.rubrik {
  font-weight: bold;
}

#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width: 1920px;
  height: 1078px;
  cursor: crosshair;
  background-image: url("/img/polacks.jpg");
}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}
</style>
