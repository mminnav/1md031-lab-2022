<template>

  <body>
    <header id="header">
      <img class="headerImage" src="https://wallbangersburgers.com/images/design/promo2.jpg" alt="headerImage">
      <h1 class="headline">
        Welcome to VEGOburger
      </h1>
    </header>
    <main>
      <section id="burgerInformation">
        <h2>
          Select burger
        </h2>
        <p>
          this is where you execute burger selection
        </p>
        <div class="wrapper">
          <Burger v-for="burger in burgers" v-bind:burger="burger" v-bind:key="burger.name" v-on:orderedBurgers="addToOrder($event)"/>
        </div>
      </section>
      <section id="customerInformation">
        <form>
          <h2>Customer information</h2>
          <p>This is where you provide necessary information</p>
          <h2>Delivery information</h2>
          <h4>Full name</h4>
          <p>
            <label for="firstname"></label><br>
            <input type="text" id="firstname" v-model="firstName" required="required" placeholder="First name">
          </p>
          <p>
            <label for="lastname"></label><br>
            <input type="text" id="lastname" required="required" v-model="lastName" placeholder="Last name">
          </p>
          <h4>E-mail</h4>
          <p>
            <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
          </p>
          <!-- <h4>Street</h4>
          <p>
            <label for="Street"></label><br>
            <input type="text" id="Street" v-model="street" required="required" placeholder="Street">
          </p> 
          <h4>House</h4>-->
          <!-- <p>
            <label for="House"></label><br>
            <input type="number" id="House" v-model="house" required="required" placeholder="House number">
          </p> -->
          <h4>Payment</h4>
          <p>
            <label for="Payment">Payment method</label><br>
            <select id="payment" v-model="payment">
              <option>Card</option>
              <option>Swish</option>
              <option>Cash</option>
            </select>
          </p>
          <h4>Gender</h4>
          <p>
            <label for="Gender">Female</label><br>
            <input type="radio" id="female" v-model="gender" required="required" placeholder="gender" value="female">
          </p>
          <p>
            <label for="Gender">Male</label><br>
            <input type="radio" id="male" v-model="gender" required="required" placeholder="gender" value="male">
          </p>
          <p>
            <label for="Gender">Do not want to denounce</label><br>
            <input type="radio" id="no" v-model="gender" required="required" placeholder="gender" value="no">
          </p>
        </form>
        <div class="container">
          <div id="map" v-on:click="setLocation">
            <div id="dots">
              <div v-bind:style="{
                left: location.x + 'px',
                top: location.y + 'px'

              }">
              T
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <div id="submit">
      <button v-on:click="submit()" type="submit">Submit</button>
      <img class="submitButtonPicture"
        src="https://st3.depositphotos.com/5483844/14410/v/450/depositphotos_144101443-stock-illustration-cartoon-hamburger-character-thumb-up.jpg"
        alt="happyHamburger">
    </div>
    <hr>
    <footer>
      &#169; 2022 VEGOburger
    </footer>
  </body>
  <!--<div>
    <div>
      Burgers
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>-->
</template>

<script>

import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();





//Object Constructor Function

/*function MenuItem(n, url, lac, glu, con, kCal) {
  this.name = n; // The *this* keyword refers to the object itself
  this.image = url;
  this.lactose = lac;
  this.gluten = glu;
  this.contains = con;
  this.calories = kCal;
}*/

// Objects are then instantiated using the *new* keyword

//const burgerList = [new MenuItem('Jalapeño burger', 'https://www.landleyskok.se/wp-content/uploads/2017/02/vegoburgare-D14I1771.jpg', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-dairy-2-512.png', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-gluten-512.png', '500 kCal'), new MenuItem('Beetroot burger', 'https://biancazapatka.com/wp-content/uploads/2021/10/rote-bete-burger.jpg', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-dairy-2-512.png', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-gluten-512.png', '500 kCal'), new MenuItem('Bean burger', 'https://eu-central-1.linodeobjects.com/tasteline/2019/11/vegoburgare-2048x2048.jpg', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-dairy-2-512.png', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-gluten-512.png', '500 kCal'), new MenuItem('Siracha burger', 'https://assets.icanet.se/e_sharpen:80,q_auto,dpr_1.25,w_1200,h_1200,c_lfill/imagevaultfiles/id_215108/cf_259/vegoburgare_med_avokado_och_srirachadressing.jpg', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-dairy-2-512.png', 'https://cdn3.iconfinder.com/data/icons/food-product-labels/128/contains-gluten-512.png', '500 kCal')];

//console.log(burgerList);

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      firstName: '',
      lastName: '',
      email: '',
      // street: '',
      // house: '',
      payment: 'Card',
      gender: 'no',
      orderedBurgers: {},
      location:{
        x: 0,
        y: 0
      }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addToOrder: function (event) {
      console.log("I addToOrder")
      this.orderedBurgers[event.name] = event.amount;
      console.log("event.amount: " + event.amount);
    },

    submit: function () {
      console.log("I submit")
      console.log(this.firstName, this.lastName, this.email, this.payment, this.gender)
      console.log(this.orderedBurgers)

      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y
        },
        orderItems: this.orderedBurgers,
        customerFirstName: this.firstName,
        customerLastName: this.lastName,
        customerEmail: this.email,
        // customerStreet: this.street,
        // customerHouse: this.house,
        customerPayment: this.payment,
        customerGender: this.gender
      }
      ); 
    },

      setLocation: function (event){
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y
      };
    },
  }
}



</script>

<style>
body {
  font-family: Courier New;
  /*font-size: 4mm;*/
}

#header {
  margin: 75px;
}

h1.headline {
  color: rgb(246, 216, 47);
  font-size: 75px;
  position: absolute;
  margin-top: -250px;
  margin-left: 150px;
}

img.headerImage {
  opacity: 0.6;
  object-fit: cover;
  width: 100%;
  height: 400px;
  object-position: center 100%;
  /*margin: 75px;*/
}

.wrapper {
  display: grid;
  grid-gap: 30px;
  grid-template-columns: 600px 600px;
  /*background-color: #fff;*/
}

/*.jalapenoBurger {
    grid-column: 1 / span 2;
}

.beetrootBurger {
    grid-column: 3 ;
    grid-row: 1 / span 2;
}

.beanBurger {
    grid-column: 1 ;
    grid-row: 2 ;
}

.sirachaBurger {
    grid-column: 2 ;
    grid-row: 2 ;
}*/

#customerInformation {
  /*border: 20px dashed white;*/
  border: 5px dashed white;
  margin: 75px;
  padding: 10px;
  background-color: black;
  color: #FFFFFF;
}

/*div{
    text-align: left;
    display: inline-block;
    background-color: aqua;
    margin: 10px;
    padding: 20px;
}
*/

img.submitButtonPicture {
  width: 50px;
  height: 50px;
}

#submit {
  margin: 100px 20px;
  background-color: #FFFFFF;
  color: #010000;
  /*border-radius: 15px;
  border: 2px solid #25c80c;*/
}

button:hover {
  cursor: pointer;
  background-color: #25c80c;
}

#burgerInformation {
  /*border: 5px dashed black;*/
  text-align: center;
  margin: 75px;
  padding: 10px;
  background-color: pink;
}

#map {
  width: 1920ox;
  height: 1078px;
  background: url("/public/img/polacks.jpg");
}

.container{
  width: 100%;
  height: 500px;
  overflow: scroll
}
</style>