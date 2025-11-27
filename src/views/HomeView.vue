<template>
<div>
    <div>
    <h1>Burgers</h1>
    <Burger v-for="burger in BurgerArray"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
    click here
    </div>
</div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

//task 1 nedan, skapa en object constructor function "MenuItem" m namn,url,kcal,allergener

function MenuItem(productName,imgUrl, kCal, hasLactose, hasGluten) {
  this.name = productName;
  this.url = imgUrl;
  this.kCal = kCal;
  this.lactose = hasLactose; 
  this.gluten = hasGluten; 
}

const BurgerArray = [
  new MenuItem("Tastys Original Burger", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQA3CxLQevdy_JdEifmfx2gZ9DN8YKL04PW3g&s", 610 ,false,false),
  new MenuItem("Tastys Very Cheesy Cheeseburger", "https://sargento.com/assets/Uploads/Recipe/Image/BonanzaBurger__FocusFillWyIwLjAwIiwiMC4wMCIsODAwLDQ3OF0_CompressedW10.jpg", 1080 ,true,true),
  new MenuItem("Tastys Healthy Burger", "https://img.taste.com.au/i4-4vuh9/taste/2022/12/no-bun-burgers-183842-2.jpg", 460 ,false,true),
]

console.log(BurgerArray)


//exempel från educora
function Employee(fn, ln, branch, pos) {
    this.firstName = fn; // The *this* keyword refers to the object itself
    this.lastName = ln;
    this.branch = branch;
    this.position = pos;
    this.name = function() {
        return this.firstName + ' ' + this.lastName;
    };
}
// slut exempel

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      BurgerArray
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}


</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>