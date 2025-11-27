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

<!-- nedan kommer från index-noscrpt.html-->   
    <header >
    <h1>Welcome to Tasty Burgers</h1>
    </header>

    <main>

        <section id = "burgersection">

            <!--<h2> Our current selection of burgers </h2>-->

   <!--     <div style="display:inline-block" -->
    <h2> Our current selection of burgers </h2>
    
            <div class="wrapper">
            

            <!--den naturella-->
            <div class="box a">
            <h3> Tastys Original Burger </h3>
            <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQA3CxLQevdy_JdEifmfx2gZ9DN8YKL04PW3g&s" title="Original" >
            <ul>
                <li><span class="allergen">Lactose-free</span></li>
                <li><span class="allergen">Gluten-free</span></li>
            </ul>
            </div>
        

            <!--den ostiga-->
            <div class="box b">
            <h3> Tastys Very Cheesy Cheeseburger </h3>
            <img src="https://sargento.com/assets/Uploads/Recipe/Image/BonanzaBurger__FocusFillWyIwLjAwIiwiMC4wMCIsODAwLDQ3OF0_CompressedW10.jpg" title="Cheesy" >
            <ul>
                <li>Contains <span class="allergen">Lactose</span></li>
                <li>Contains <span class="allergen">Gluten</span></li>
        
            </ul>
            </div>



            <!--salladsburgaren-->
            <div class="box c">
            <h3> Tastys Healthy Burger </h3>
            <img src="https://img.taste.com.au/i4-4vuh9/taste/2022/12/no-bun-burgers-183842-2.jpg" title="Healthy" >
            <ul>
                <li><span class="allergen">Lactose-free</span></li>
                <li>Contains <span class="allergen">Gluten</span></li>
                <li>Vegetarian Friendly</li>  
                <li>This burger comes without bread</li>     
            </ul>
            </div>
    
        </div>
        
            </section>

       <section id="chooseburger">
            <h4> Select burger </h4>
            <p>
   <label for="chosenburger">Choose burger</label><br>
   <select id="chosenburger" name="chosenburger">
        <option selected="selected">Select burger</option>
       <option>Tastys Original Burger</option>
       <option>Tastys Very Cheesy Cheeseburger</option>
       <option>Tastys Healthy Burger</option>
   </select>
</p>
        </section>

    <section id="orderinfo">
    <h2> Order information </h2>
    <legend><h4> Personal information</h4></legend>
        
<!--<fieldset>-->

    
    <form>
        <p>
    <label for="fullname">Full name</label><br>
    <input type="text" id="fullname" name="fullname" required="required" placeholder="First and last name">
    </p>
<p>
    <label for="house adess">Full address</label><br>
    <input type="text" id="streetaddress" name="streetaddress" required="required" placeholder="Street">
    <input type="number" id="housenumber" name="housenumber" required="required" placeholder="Number"><br>
    <input type="text" id="postal" name="postal" required="required" placeholder="Postal code">
</p>
     
    
    <p>
    <label for="gender">Gender:</label>

    <input type="radio" id="male" name="gender" value="male">
    <label for="male">Male</label>

    <input type="radio" id="female" name="gender" value="female">
    <label for="female">Female</label>

    <input type="radio" id="other" name="gender" value="other" />
    <label for="other">Other</label>
</p>

<p>
   <label for="paymentmethod">Payment Method</label><br>
   <select id="paymentmethod" name="paymentmethod">
       <option selected="selected">Swish</option>
       <option>Card Payment</option>
       <option>Klarna</option>
       <option>Cash to deliverer</option>
   </select>
</p>

</form>
<!--</fieldset>-->
</section>

<section id="thesubmitbutton">
    
  <button type="submit" name="Submit" >Submit order!</button>

</section>

    
    </main>

    <hr>

    <footer>
        Nominated Best Burger of the Year 2025. <br/>
        For complaints please email complaints@tastyburgers.com
    </footer>


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