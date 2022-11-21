<template>

  <body>
    <section>
      <header>
        <img
          src="https://www.gallerian.se/contentassets/667a269d11524a339d2153c2274a16e3/bastard-burgers-header.jpg?preset=16x8-1200&version=63697923645"
          alt="">
        <h1> Welcome to Burger place</h1>
      </header>
    </section>
    <main>
      <section id="Hamburgers">
        <h2>Selcet a burger</h2>
        <p>This is where you execute burger selection
        </p>
        <div class="wrapper">
          <Burger v-for="burger in burgers" v-bind:burger="burger" v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)" />
          <p>Amount ordered:</p>
        </div>

      </section>
      <section id="customer">
        <h2>Customer information</h2>
        <p>This is where you provide neccessary information</p>

        <p>
          <label for="fullname">First- and Last Name </label><br>
          <input type="text" id="fullname" v-model="na" required="required" placeholder="Full name">
        </p>
        <p>
          <label for="email">Email</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        </p>
        <div class="mapScroll">
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

        <p>
          <label for="paymentmethods">Payment methods</label><br>
          <select id="payment" v-model="pmt">
            <option value="Card">Card</option>
            <option value="Cash">Cash</option>
            <option value="Swish">Swish</option>
            <option value="Innvoice">Innvoice</option>
          </select>
        </p>

        <p>Gender</p>

        <input type="radio" id="female" value="Female" v-model="gender" required="required">
        <label for="female">Female </label>
        <br />

        <input type="radio" id="male" value="Male" v-model="gender" required="required">
        <label for="male">Male </label>
        <br />

        <input type="radio" id="dwp" value="Other" v-model="gender" required="required">
        <label for="dwp">Do not wish to provide </label>


      </section>
    </main>

    <button type="submit" v-on:click="submit">
      <img
        src="https://w7.pngwing.com/pngs/107/673/png-transparent-hamburger-barbecue-cheeseburger-barbecue-food-cheeseburger-cartoon.png">
      Place order
    </button>

    <footer>
      &copy 2018 vendelas burger
    </footer>
  </body>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io();

function MenuItem(n, url, kcal, lact, glut) {
  this.name = n; // The *this* keyword refers to the object itself
  this.url = url;
  this.kcal = kcal;
  this.lactose = lact;
  this.gluten = glut;

}

const list_of_burgers = [new MenuItem('Fireburger', 'https://images.ohmyhosting.se/ie1i7Wt-F3j_nOJfadWoldwQ6iQ=/804x1078/smart/filters:quality(85)/https%3A%2F%2Fbastardburgers.com%2Fwp-content%2Fuploads%2Fsites%2F6%2F2020%2F12%2Fwebb-_0000s_0016_new-york.jpg.jpg', '500', 'true', 'true'),
new MenuItem('Chickenburger', 'https://images.ohmyhosting.se/P5kVzNntfS_wg4c2XcSsY4mlUhA=/804x1078/smart/filters:quality(85)/https%3A%2F%2Fbastardburgers.com%2Fwp-content%2Fuploads%2Fsites%2F6%2F2022%2F01%2Fwebb-_0000s_0017_no-chicken-chicago.jpg.jpg', '500', 'false', 'true'),
new MenuItem('Veganburger', 'https://images.ohmyhosting.se/iHq3oKzy8yawj0Xl6gpRHrXkSFc=/804x1078/smart/filters:quality(85)/https%3A%2F%2Fbastardburgers.com%2Fwp-content%2Fuploads%2Fsites%2F6%2F2022%2F01%2Fwebb-_0000s_0001_GO-london.jpg.jpg', '500', 'true', 'true')];

export default {
  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: menu,
      na: '',
      em: '',
      gender: '',
      pmt: 'Swish',
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      }
    }
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },

    submit: function () {
      console.log(this.na, this.em, this.gender, this.pmt)
      console.log(this.orderedBurgers.value)

      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y
        },
        orderItems: this.orderedBurgers,
        customerNa: this.na,
        customerEm: this.em,
        customerGender: this.gender,
        customerPmt: this.pmt
      }
      );
      

    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },

    setLocation: function (event) {
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
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}

footer {
  border-top: 2px solid black;
}

section {
  margin: 30px;
}

header img {
  position: relative;
  width: 100%;
  height: auto;
  opacity: 0.5;
}

header {
  height: 300px;
  overflow: hidden;

}

.ingredients {
  font-weight: bold;
}

#Hamburgers {

  color: white;
  background-color: black;
  border: 2px dashed;
  padding: 10px;

}

#customer {
  background-color: white;
  border: 2px dashed;
  padding: 10px;

}

button:hover {
  background-color: rgb(160, 230, 255);
  cursor: pointer
}

button {
  margin: 20px;
}

button img {
  width: 50px;

}

header h1 {
  font-size: 60px;
  position: absolute;
  top: 50%;
  width: 100%;
  text-align: center;
  margin-top: -190px;
  margin-left: -50px;

}

.wrapper {
  display: grid;
  grid-gap: 30px;
  grid-template-columns: 400px 400px 400px;


}

.wrapper img {
  width: 400px;
}

.box {

  border-radius: 5px;
  padding: 20px;
  margin-left: 20px;
  font-size: 150%;
}

#map {

  width: 1920px;
  height: 1078px;
  background: url('../../public/img/polacks.jpg');
}

.mapScroll {
  width: 100%;
  height: 500px;
  overflow: scroll;
}

#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width: 1920px;
  height: 1078px;
  cursor: crosshair;
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