<template>
  <div>
    <h3>{{ burger.name }}</h3>
    <img class="pics" v-bind:src="burger.img">
    <span id="ingredients">
      <ul>
        <li> Calories: {{ burger.calories }}</li>
        <li> Contains lactose: {{ burger.lactoes }}</li>
        <li> Contains gluten: {{ burger.gluten }}</li>
      </ul>
    </span>

    {{ amountOrdered }}

    <button type="submit" style="width: 30px" v-on:click="addBurger()">
      <img src="../../public/img/addpic.png" style="width: 10px;">
    </button>

    <button type="submit" style="width: 30px" v-on:click="removeBurger()">
      <img src="../../public/img/subsign.png" style="width: 10px;">
    </button>

    <button type="submit" style="width: 60px" v-on:click="sendOrder()">
      ADD TO ORDER 
    </button>

  </div>
</template>


<script>
import { createConditionalExpression } from '@vue/compiler-core';

export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },

  data: function () {
    return {
      amountOrdered: 0
    }
  },

  methods: {
    addBurger: function () {
      this.amountOrdered += 1;
    },
    removeBurger: function () {
      if (this.amountOrdered != 0) {
        this.amountOrdered -= 1;
      }
    },
    sendOrder: function () {
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      });
    }
  }
}

</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
  