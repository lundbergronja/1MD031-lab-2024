<template>

  <div class="box">

  <h3> {{ burger.name }} </h3>
  <img v-bind:src="burger.url" style="width: 33%">
        <ul>
        <li v-if="burger.gluten == true"> Contains Gluten </li>
        <li v-else> Gluten-free </li>
        <li v-if="burger.lactose == true"> Contains Lactose </li>
        <li v-else> Lactose-free</li>
        <li> {{  burger.protein  }} </li>

        </ul>

  </div>


</template>



<script>

export default {
  name: 'OneBurger',
  props: {
    burger: Object
  }
}

</script>




<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

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


</style>