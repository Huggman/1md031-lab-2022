<template>
<div>
  <header>
        <img id="headerimg" src = "https://ifkgoteborg.se/wp-content/uploads/2022/05/Lejon_LP.jpg" alt = "Span">
        <h1 id = "intro">Välkommen till Göteburgaren</h1>
      </header>
    <main>
        <div id="hamburgare">
          <h2> Vår meny </h2>
            <div class="wrapper">
            <Burger v-for="burger in burgers"
                    v-bind:burger="burger"
                    v-bind:keys="burger.name"
                    v-on:orderedBurgers="addToOrder($event)"
                    />
              </div>
          </div>


        <section id="leveransinformation">
            <div>
                <h2>Leveransinformation</h2>
            </div>
            <form>
                <p>
                    <label for="firstname">First Name</label><br>
                    <input type="text" id="firstname" v-model="fn" required="required" placeholder="First name">
                </p>
                <p>
                    <label for="lastname">Last Name</label><br>
                    <input type="text" id="lastname" v-model="ln" required="required" placeholder="Last name">
                </p>
                <p>
                    <label for="email">Email adress</label><br>
                    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                </p>
              <!--   <p>
                    <label for="street">Street</label><br>
                    <input type="text" id="street" v-model="st" required="required" placeholder="Street">
                </p>
                <p>
                    <label for="housenumber">House Number</label><br>
                    <input type="number" id="housenumber" v-model="hn" required="required" placeholder="House Number">
                </p> -->
        

            <p>
                <label for="recipient">Betalning</label><br>
                <select id="recipient" v-model="rcp">
                    <option>Visa</option>
                    <option>MasterCard</option>
                    <option>Swish</option>
                    <option>Apple Pay</option>
                    <option>Betala vid utkörning</option>
                </select>
             </p>
        
             <p>
                <label for="ge">Kön:</label><br>
                <input type="radio" id="Male" v-model="ge" value="Male" required="required"> 
                <label for="Male">Man</label><br>
                <input type="radio" id="Female" v-model="ge" value="Female" required="required">
                <label for="Female">Kvinna</label><br>
                <input type="radio" id="Other" v-model="ge" value="Other" required="required">
                <label for="Other">Annat</label><br>
                <input type="radio" id="Vill ej ange" v-model="ge" value="Vill ej ange" required="required">
                <label for="Vill ej ange">Vill ej ange</label>
            </p>   
            <button id="knapp" type="submit" v-on:click="printOrder">
            </button>
            </form>
        </section>
        


     </main>

  <div id="mapDiv">
    <div id="map" v-on:click="setLocation">
      <div v-bind:style="{left:location.x+'px', top: location.y+'px'}">
      T
      </div>
    </div>
  </div>
  <footer>
        <hr>
        &copy Huggman AB
     </footer>
  
</div> 
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();
//const menu = JSON.parse();


  /* function MenuItem(nm, kC, url, glu, lak) {
  this.name = nm;
  this.kCal = kC;
  this.pic = url;
  this.gluten = glu;
  this.laktos = lak;
}

const burger1 = new MenuItem("Glenns goa", 100, "https://img.koket.se/standard-mega/klassisk-amerikansk-hamburgare.jpg", true, false)
const burger2 = new MenuItem("Änglarna", 250, "https://img.koket.se/standard-mega/klassisk-amerikansk-hamburgare.jpg", false, false)
const burger3 = new MenuItem("Frölunda", 200, "https://img.koket.se/standard-mega/klassisk-amerikansk-hamburgare.jpg", true, true)

const burger = [burger1, burger2, burger3];  */
//console.log(burger)
//console.log(menu)

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fn: '',
      ln: '',
      em: '',
      rcp: '',
      ge: '',
      
      orderedBurgers: {},
      location: {
        x:0,
        y:0
      }
    }
  },
  methods: {
    getSelectedBurger: function() {
      this.selectedBurger = event;
    },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      console.log(event)
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y},
                                orderItems: []
                              }
                 );
                 this.location.x=event.clientX - 10 - offset.x;
                 this.location.x=event.clientY - 10 - offset.y;
    },
    printOrder: function () {
      console.log(this.fn, this.ln, this.em, this.rcp, this.ge, this.orderedBurgers)
      console.log(this.orderedBurgers);
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
      details: {
        x: this.location.x,
        y: this.location.y
      },
      orderItems: this.orderedBurgers,
      customerInformation:[this.fn, this.ln, this.em, this.rcp, this.ge]
    });
      },

  setLocation: function (event) {
      this.location.x=event.clientX - 10 - event.currentTarget.getBoundingClientRect().left;
      this.location.y=event.clientY - 10 - event.currentTarget.getBoundingClientRect().top;
    },
    addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
  },
    


  }
}
</script>

<style>
  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
body {
    font-family: arial;
    background-color: #6b7ff1;
 }

 .textcolor {
    color: #f7ff08;
    display: inline-block;
    font-weight: bold;
 }


#headerimg {
   opacity: 0.5;
   height: 250px;
   width: 100%;
   overflow:hidden;
}

#intro {
  -webkit-text-stroke: 1px black;
  font-family: sans; color: yellow;
  position:absolute; 
  padding: 10%; 
  margin-top: -20%; 
  margin-left: 25%;
}

h1 {
   position:absolute;
}

.wrapper {
     display: grid;
     grid-gap: 1%;
     grid-auto-flow: column; 
     grid-template-columns: 33% 33% 33%;
 }

 #burgerpic {
  height: auto;
  width: auto;
 }

 .box {
     background-color: #444;
     color: #fff;
     border-radius: 5px;
     padding: 20px;
     font-size: 150%;
 }


 #hamburgarefont {
    background-color: rgb(19, 15, 226);
    color:rgb(250, 244, 236);
    margin: 15px 25px 25px 25px;
 }

 #hamburgare {
    border: 5px dashed #fefefe;
    border-radius: 15px;
    background-color: rgb(19, 15, 226);
    color: antiquewhite;
 }
 
 #leveransinformationheader {
   background-color: rgb(19, 15, 226);
   color: rgb(250, 244, 236);
}

#leveransinformation {
   border: 5px dashed rgb(19, 15, 226);
   border-radius: 15px;
   background-color: rgb(246, 240, 232);
   color: rgb(19, 15, 226);
}
 
 button:hover {
    background-color:rgb(83, 81, 223);
    color: antiquewhite;
    font-weight: bold;
 }

 #knapp {
    margin-left: 0.5%;
    margin-bottom: 0.5%;
    cursor:pointer;
    font-weight: bold;
    width: 150px;
    height: 100px;
    background-image: url("C:/Users/Hugge/Documents/GitHub/1md031-lab-2022/public/img/valle.jpg");
    background-size: 100% 100%;
  }

  #mapDiv {
    margin-left: 0.5%;
    width: 500px;
    height: 500px;
    overflow:scroll;
    position:relative;
  }

  #map {
    width: 1920px;
    height: 1078px;
    background: url("C:/Users/Hugge/Documents/GitHub/1md031-lab-2022/public/img/polacks.jpg");
    position:absolute;
    background-repeat: no-repeat;
    cursor: pointer
  }

  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
</style>