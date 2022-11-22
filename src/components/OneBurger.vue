<template>
  <div class="box">
    <h3>
      {{ burger.name }}
    </h3>
    <img class="burgerPictures" v-bind:src="burger.image" alt="hamburger">
    <dl>
      <dt class="ingredients">
        <img class="alergies" v-bind:src="burger.lactose" alt="containsLactose">
        <img class="alergies" v-bind:src="burger.gluten" alt="containsGluten">
      <dd> {{ burger.contains }} </dd>
      <dd> {{ burger.calories }} </dd>
      </dt>
    </dl>
    <div class="burgerAmount">
      <button v-on:click="remove">-</button>  {{ amountOrdered }}
      <button v-on:click="add">+</button>
    </div>
  </div>
</template>
  
<script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    data: function () {
      return {
        amountOrdered: 0,
      }
    },

    methods: {
      add: function () {
        this.amountOrdered++;
        this.$emit('orderedBurgers',
          {
            name: this.burger.name,
            amount: this.amountOrdered
          }
        );
      },

      remove: function () {
        if (this.amountOrdered > 0) {
          this.amountOrdered--;
        }
        this.$emit('orderedBurgers',
          {
            name: this.burger.name,
            amount: this.amountOrdered
          }
        )

      },
    }
  }
</script>

<style scoped>
  img.burgerPictures {
    object-fit: cover;
    width: 525px;
    height: 400px;
    object-position: center 50%;
  }

  .ingredients {
    text-align: left;
  }

  img.alergies {
    text-align: left;
    width: 30px;
    height: 30px;
  }

  .box {
    background-color: rgb(248, 229, 121);
    color: black;
    padding: 20px;
    font-size: 150%;
    margin: 10px;
    margin-left: 30px;
  }
</style>
  