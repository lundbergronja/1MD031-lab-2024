<template>
  <header>
    <h1>Welcome to Tasty Burgers</h1>
  </header>

  <div>
    <h2>Our current selection of burgers</h2>
    <h4>Use the +/- buttons to build your order</h4>
    <div class="wrapper">
      <Burger
        v-for="burger in BurgerArray"
        v-bind:burger="burger"
        v-bind:key="burger.name"
        v-on:orderedBurger="addToOrder($event)"
      />
    </div>

    <!-- nedan kommer från index-noscrpt.html-->

    <main>
      <section id="orderinfo">
        <h2>Order information</h2>
        <legend><h4>Please provide your personal information here</h4></legend>

        <form>
          <p>
            <label for="fullname">Full name</label><br />
            <input
              type="text"
              id="fullname"
              v-model="fullname"
              required="required"
              placeholder="First and last name"
            />
          </p>
          <p>
            <label for="gender">Gender:</label>

            <input type="radio" id="male" v-model="gender" value="male" />
            <label for="male">Male</label>

            <input type="radio" id="female" v-model="gender" value="female" />
            <label for="female">Female</label>

            <input type="radio" id="other" v-model="gender" value="other" />
            <label for="other">Other</label>
          </p>
          <p>
            <label for="paymentmethod">Payment Method</label><br />
            <select id="paymentmethod" v-model="paymentmethod">
              <option selected="selected">Swish</option>
              <option>Card Payment</option>
              <option>Klarna</option>
              <option>Cash to deliverer</option>
            </select>
          </p>
        </form>
      </section>

      <div class="delivery">
        <p>Please pin your drop-off location on the map:</p>
        <div id="map" v-on:click="setLocation">
          <div
            class="target"
            v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }"
          >
            T
          </div>
        </div>
      </div>

      <section id="thesubmitbutton">
        <button type="submit" v-on:click="submitOrder">Submit order!</button>
      </section>

      <!--nedan är stora röda boxen-->
    </main>
    <hr />
    <!--detta är vita linjen över footern-->
    <!--nedan är footern-->
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

//min constructor function "MenuItem", som inte används längre
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
  //mina komponenter
  components: {
    Burger,
  },

  //min datastruktur
  data: function () {
    return {
      BurgerArray,
      chosenburger: "",
      fullname: "",
      gender: "",
      paymentmethod: "",
      orderedBurgers: {},
      location: { x: 0, y: 0 },
    };
  },

  //mina metoder/funktioner
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    submitOrder: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: { x: this.location.x, y: this.location.y },
        orderItems: this.orderedBurgers,
        customerInfo: {
          customerName: this.fullname,
          gender: this.gender,
          paymentMethod: this.paymentmethod,
        },
      });
      console.log("Ordered burger:", this.chosenburger);
      console.log("Full name:", this.fullname);
      console.log(
        "Delivery address:",
        this.streetaddress,
        this.housenumber,
        ",",
        this.postal,
        this.city
      );
      //ev ta med det under detta
      console.log("PIN 4 delivery:", this.location);
      console.log(this.orderedBurgers);
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location = {
        x: event.clientX - offset.x,
        y: event.clientY - offset.y,
      };
    },
  },
};
</script>

<style>
.delivery {
  width: 100%;
  height: 400px;
  overflow: scroll;
}
.delivery p {
  color: black;
  font-weight: bold;
}

#map {
  position: relative;
  width: 1920px;
  height: 1078px;
  background: url("/img/polacks.jpg");
  background-size: initial;
  background-repeat: no-repeat;
}
.target {
  position: absolute;
  width: 20px;
  height: 20px;
  background: black;
  color: white;
  border-radius: 10px;
  text-align: center;
  line-height: 20px;
  font-weight: bold;
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
  color: #eec2c2;
  font-size: larger;
  border: 2px dotted darkred;
  padding-left: 20px;
}
#orderinfo h4 {
  color: black;
  font-size: smaller;
}

/*select gäller min select burger
select {
}*/

button:hover {
  background-color: #eec2c2;
  color: crimson;
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
}
</style>
