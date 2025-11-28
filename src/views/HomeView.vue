
<template>

    <header >
    <h1>Welcome to Tasty Burgers</h1>
    </header>

<div>

    <div>

    <h2> Our current selection of burgers </h2>

    <Burger v-for="burger in BurgerArray"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
    </div>

    <div id="map" v-on:click="addOrder">
    click here
    </div>

<!-- nedan kommer från index-noscrpt.html-->   

    <main>

        <section id = "burgersection">

    
           <!--  <div class="wrapper">--> 
    
        
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
        


    
    <form>
        <p>
    <label for="fullname">Full name</label><br>
    <input type="text" id="fullname" name="fullname" required="required" placeholder="First and last name">
    </p>
<p>
    <label for="fulladress">Full address</label><br>
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
</div>

</template>

<script>
import menu from '../assets/menu.json'
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

//task 1 nedan, skapa en object constructor function "MenuItem" m namn,url,kcal,allergener

function MenuItem(productName,imgUrl, kCal, hasLactose, hasGluten, protein) {
  this.name = productName;
  this.url = imgUrl;
  this.kCal = kCal;
  this.lactose = hasLactose; 
  this.gluten = hasGluten;
  this.protein = protein 
}

const BurgerArray = menu;



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
/* nedan kommer från min css-fil */

@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

/* Här testar jag att kommentera */

body {
    font-size: 10pt;
   font-family: "Courier New", sans-serif; /* Note: Gillar också Open Sans, sans-serif */
    background-color: indianred; /*ändrar fäg på bakgrunden bakom hela altet, floralwhite typ lite offwhite vit, indianred lite rostfärg*/
}

p {
    /*detta påverkar rubrikerna i order info*/
    color: azure;
}

h1 {
    font-family: 'Agbalumo';
    font-size: 36pt;
}

main {
  /*här var det ngn bakgrundsfäg som jag tog bort*/
}


header {
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

/*Jag tror att det jag lyckats använda flexbox-metoden för att styla headern*/


header h1 {
    width: auto;
    margin: 0 auto;
    text-align: center;

}

.allergen {
   font-weight: bold;
}

.classname {
   color: #ff5500;
}

#burgersection {
   color: white;
   background-color: black;
   border: 2px dotted darkred
}

#burgersection div {
   padding: 10px;
   margin: 20px;
}

#burgersection h2 {
   padding-left: 20px;
   color: crimson
}

#orderinfo {
    color: black;
   border: 2px dotted darkred;
   padding-left: 20px;
}



button:hover {
   background-color: floralwhite;
   cursor: pointer;
}

button {
   margin: 30px;
}

section{
    margin: 20px 20px 20px 20px;
}

/*Nedan börjar labb-task kring grid*/

.wrapper {
     display: grid;
     grid-gap: 5px;
     grid-template-columns: 33% 33% 33%;
     /*background-color: #fff; känns som denna bara gjorde det fulare*/
     color: #eec2c2;
 }

 .wrapper img {
    width: 100%;
    height: auto;
}

 .box {
     background-color: crimson;
     color: black; /*textfärg, overridar wrapper ovan*/
     border-radius: 30px;
     padding: 20px;
     font-size: 100%;
 }


 .a {
     grid-column: 1 ;
     grid-row: 1 ;
 }
 .b {
     grid-column: 2 ;
     grid-row: 1 ;
 }
 .c {
     grid-column: 3 ;
     grid-row: 1 ;
 }

</style>