<template>
  <div>
    <div class="box">
      <h2>{{ burger.name }}</h2>
      <img v-bind:src="burger.url" alt="">
      <ul>
        <li> {{ burger.kCal }} kCal </li>
        <li v-if="burger.lactose" >Contains <span class="ingredients"> lactose </span> </li>
        <li v-if="burger.gluten">Contains <span class="ingredients"> gluten </span> </li>
      </ul>

      <p>
        Amount: 
        <button v-on:click="removeBurger"> - </button>
        {{amountOrdered}}
        <button v-on:click="addBurger"> + </button>
      </p>
    </div>
  </div>
</template>
  
<script>

export default {
  data: function () {
    return {
      amountOrdered: 0,
    }
  },

  name: 'OneBurger',
  props: {
    burger: Object
  },

  methods: {
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      }
      );
    },
    removeBurger: function () {
      if(this.amountOrdered>0){
        this.amountOrdered -= 1;
      }
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      }
      );
    },
  }

}

</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper img {
  width: 400px;
}

.box {

  border-radius: 5px;
  padding: 20px;
  margin-left: 20px;
  font-size: 150%;
}

.amount {
  display: inline-block;
  font-size: 20px
}
.box p{
  font-size: 20px;
}
.box li {
  font-size: 20px;
}
</style>
  