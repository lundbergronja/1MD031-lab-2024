<template>
  <header>
    <h1>Welcome to Tasty Burgers</h1>
  </header>

  <div>
    <h2>Our current selection of burgers</h2>
    <div class="wrapper">
      <Burger
        v-for="burger in BurgerArray"
        v-bind:burger="burger"
        v-bind:key="burger.name"
      />
    </div>

    <div id="map" v-on:click="addOrder">click here</div>

    <!-- nedan kommer från index-noscrpt.html-->

    <main>
      <section id="chooseburger">
        <h4>Select burger</h4>
        <p>
          <label for="chosenburger">Choose burger</label><br />
          <select id="chosenburger" name="chosenburger">
            <option selected="selected">Select burger</option>
            <option>Tastys Original Burger</option>
            <option>Tastys Very Cheesy Cheeseburger</option>
            <option>Tastys Healthy Burger</option>
          </select>
        </p>
      </section>

      <section id="orderinfo">
        <h2>Order information</h2>
        <legend><h4>Personal information</h4></legend>

        <form>
          <p>
            <label for="fullname">Full name</label><br />
            <input
              type="text"
              id="fullname"
              name="fullname"
              required="required"
              placeholder="First and last name"
            />
          </p>
          <p>
            <label for="fulladress">Full address</label><br />
            <input
              type="text"
              id="streetaddress"
              name="streetaddress"
              required="required"
              placeholder="Street"
            />
            <input
              type="number"
              id="housenumber"
              name="housenumber"
              required="required"
              placeholder="Number"
            /><br />
            <input
              type="text"
              id="postal"
              name="postal"
              required="required"
              placeholder="Postal code"
            />
          </p>

          <p>
            <label for="gender">Gender:</label>

            <input type="radio" id="male" name="gender" value="male" />
            <label for="male">Male</label>

            <input type="radio" id="female" name="gender" value="female" />
            <label for="female">Female</label>

            <input type="radio" id="other" name="gender" value="other" />
            <label for="other">Other</label>
          </p>

          <p>
            <label for="paymentmethod">Payment Method</label><br />
            <select id="paymentmethod" name="paymentmethod">
              <option selected="selected">Swish</option>
              <option>Card Payment</option>
              <option>Klarna</option>
              <option>Cash to deliverer</option>
            </select>
          </p>
        </form>
      </section>

      <section id="thesubmitbutton">
        <button type="submit" name="Submit">Submit order!</button>
      </section>
    </main>

    <hr />

    <footer>
      Nominated Best Burger of the Year 2025. <br />
      For complaints please email complaints@tastyburgers.com
    </footer>
  </div>
</template>

<script>
import menu from "../assets/menu.json";
import Burger from "../components/OneBurger.vue";
import io from "socket.io-client";

const socket = io("localhost:3000");

//min constructor function "MenuItem"
function MenuItem(productName, imgUrl, kCal, hasLactose, hasGluten, protein) {
  this.name = productName;
  this.url = imgUrl;
  this.kCal = kCal;
  this.lactose = hasLactose;
  this.gluten = hasGluten;
  this.protein = protein;
}
//min array med burgare
const BurgerArray = menu;

export default {
  name: "HomeView",
  components: {
    Burger,
  },
  data: function () {
    return {
      BurgerArray,
    };
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y,
        },
        orderItems: ["Beans", "Curry"],
      });
    },
  },
};
</script>

<style>
#map {
  width: 300px;
  height: 300px;
  background-color: red;
}

@import url("https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap");

body {
  font-size: 10pt;
  font-family: "Courier New", sans-serif; /* Note: Gillar också Open Sans, sans-serif */
  background-color: indianred; /*ändrar fäg på bakgrunden bakom hela altet, floralwhite typ lite offwhite vit, indianred lite rostfärg*/
}

header {
  /*detta påverkar min header (och även welcome rubrik)*/
  background-image: url("https://i.pinimg.com/originals/58/76/de/5876de05f919a8a5b94b35c16d2d231a.gif");
  background-size: cover;
  overflow: hidden;
  width: 100%;
  height: 275px;
  opacity: 0.85;
  margin-top: 30px;
  /*Detta nedanför hittade jag på StackOverflow för att få outlined text https://stackoverflow.com/questions/4919076/outline-effect-to-text*/
  -webkit-text-fill-color: crimson;
  -webkit-text-stroke: 1px;
  position: relative;
}

header h1 {
  /*detta påverkar min Welcome to tasty rubrik*/
  font-family: "Agbalumo";
  font-size: 40pt;
  width: auto;
  margin: 0 auto;
  text-align: center;
}

#orderinfo {
  color: black;
  border: 2px dotted darkred;
  padding-left: 20px;
}

p {
  /*detta påverkar rubrikerna i order info*/
  color: azure;
}

#burgersection {
  color: white;
  background-color: black;
  border: 2px dotted darkred;
}

button:hover {
  background-color: floralwhite;
  cursor: pointer;
}

button {
  margin: 30px;
}

section {
  margin: 20px 20px 20px 20px;
}

.wrapper {
  /*.wrapper justerar min parent till burgarboxara, dvs bakom dom*/
  display: grid;
  grid-gap: 5px;
  grid-template-columns: 33% 33% 33%;
  color: #eec2c2;
  background-color: black;
}
</style>
