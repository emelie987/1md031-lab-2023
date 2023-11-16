<template>
      <header>
        <img src="img/header.png" class = head>
         <h1>Welcome Burger Broes</h1>
      </header>

        <main>
        <section class = "burger">
          <h2>Menu</h2>
          <div class = "wrapper">
          <Burger v-for="burger in burgers"
            v-bind:burger = "burger" 
            v-bind:key = "burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
          </div>
        </section>
        
  
        
         <section class="orderdetails">
            <h2>Order information:</h2>
            <h3>Please fill out the form for ordering</h3>
            <form>
               <p>
                  <label for="first name">Full name</label><br>
                  <input type="text" id="firstName" v-model="fn" required="required" placeholder="type here...">
               </p>
               <p>
                  <label for="Email adress">Email adress</label><br>
                  <input type="email" id="emailAdress" v-model="ea" placeholder="type here...">
               </p>
            
               <p>
                  <label for="Phone number">Phone number</label><br>
                  <input type="number" id="phoneNumber" v-model="pn" required="required" placeholder="type here...">
               </p>

               <label>
               <input type="radio" v-model="gender" name="gender" required="required" placeholder="enter gender" value="male"> Male
               </label>
               <br>
               <label>
               <input type="radio" v-model="gender" name="gender" required="required" placeholder="enter gender" value="female" checked="checked"> Female 
               </label>
               <br>
               <label>
               <input type="radio" v-model="gender" name="gender" required="required" placeholder="enter gender" value="non binary"> Non binary
               </label>
               <br>
               <label>
               <input type="radio" v-model="gender" name="gender" required="required" placeholder="emter gender" value="other"> Do not wish to provide
               </label>
               <br>

               <h2>Payment methods:</h2>
               <p>
                  <label for="payment method">Choose your payment method:</label>
                  <br>
                  <select id="payment method" v-model="pm">
                     <option>Swish</option>
                     <option>Klarna, pay now</option>
                     <option>Klarna, installment</option>
                     <option>Debit card</option>
                     <option>Credit card</option>
                  </select>
               </p>
       
               <div id="outerMap" style="height: 300px; overflow: auto;">
                <div id="innerMap" v-on:click="addOrder">
                  <div id="dots">
                    <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
                      T
                    </div>
                  </div>
                </div>
              </div>

            </form>

            <div id="orders">
            <button type="submit" v-on:click="markDone(key)">
            <img src="img/kanppp.jpg" style="width: 30px;">
            SEND ORDER
            </button>
            </div>
  </section>
  </main>



      <footer> 
      <hr> 
      <p> &copy; 2023 Emelie Edberg </p>
      <hr>
      <p> Contact us:</p>
      <hr>  
      <p>burgerbroes@gmail.com</p>
      <p>+46 73 708 67 94</p>
      <p>Rosendahls Allé 12B</p>
      </footer>

</template>

<script>
import menu from '../assets/menu.json'
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();

/*function Itmensmenu(n, kcal, url, g, l){ 
  this.name = n; 
  this.calories = kcal; 
  this.url = url; 
  this.gluten = g; 
  this.lactose = l; 
}

let burgerList = [{name: "Classic Burger", kCal: 500, url: "img/classicburger.jpg", lactoes: true, gluten: true},
{name: "Halloumi Burger", calories: 400, url: "img/hallou3.jpg", lactoes: true, gluten: true},
{name: "Chicken Burger", calories: 400, url: "img/chickenburger2.jpg", lactoes: true, gluten: true}
]

console.log(burgerList)*/



export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu, 
      fn:'',
      ea:'',
      sn:'',
      pn:'',
      orderedBurgers: {},
      location: { x: 0, y: 0 }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      this.firstName
      this.emailAdress
      this.streetName
      this.houseNumber
      this.phoneNumber

      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

                 this.location.x=event.clientX - 10 - offset.x;
                 this.location.y=event.clientY -10 - offset.y;
    }, 

    addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
    console.log(this.orderedBurgers);
    },

    setLocation: function(event){
      this.location.x=event.clientX - 10 - event.currentTarget.getBoundingClientRect().left;
      this.location.y=event.clientY - 10 - event.currentTarget.getBoundingClientRect().top;
      console.log(this.location);
    },

    markDone: function() {
      console.log(this.fn)
      console.log(this.ea)
      console.log(this.sn)
      console.log(this.hn)
      console.log(this.pn)
      console.log(this.pm)

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y },
                                orderItems: this.orderedBurgers,
                                customerInformation: { name: this.fn,
                                                       emailAdress: this.ea,
                                                       streetName: this.sn,
                                                       houseNumber: this.hn,
                                                       phoneNumber: this.pn,
                                                       paymentMethod: this.pm
                                }
                              }
                 );
    }
  }
}

</script>

<style>
  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';



  #outerMap {
    width: 100%;
    height: 40vh;
    overflow: scroll;
  }

  #innerMap {
    background-image: url("../../public/img/polacks.jpg");
    background-size: cover;
    width: 1920px;
    height: 1078px;
    cursor:grab;
  }


  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }



body {
    font-family: 'Times New Roman', Times, serif;
    color: black;
    font-size: large; 
    background-color: pink;
 }

header{
    position: relative; 
    text-align: center; 
    height: 150px; 
    margin: 80px; 
    padding-top: 20px;
   
}

.head{
    position: absolute; 
    top: 0;
    left: 0; 
    width: 100%;
    height: 100%;
    z-index: -1;
    opacity: 0.5; 
}


.wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(3, 1fr);
}


 
 .burger {
    color: whitesmoke;
    background-color: black;
    margin: 80px;
    padding: 70px; 
    border: 15px double ;
 }
 
 #ingridients {
    text-transform: uppercase;
 }

.classic_burger{
    margin: 2px; 
}

.halloumi_burger{
    margin: 2px;
} 

.chicken_burger{
    margin: 2px;
}

 .orderdetails{
    background-color: aquamarine;
    margin: 70px;
    padding: 30px; 
    border: 15px double black; 
 }

 button:hover {
    cursor: pointer;
    background-color: aquamarine;
 }

 button{
    margin: 50px; 
 }

 .pics{
height: 15em; 
width: 15em; 
 }

 #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

</style>