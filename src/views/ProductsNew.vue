<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="name"></p>
    <p>Price: <input type="text" v-model="price"></p>
    <p>Description: <input type="text" v-model="description"></p>
    <button v-on:click="createNewProduct">Create a new Product</button>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Create a New Product!",
      name: "",
      price: "",
      description: ""
    };
  },
  created: function() {},
  methods: {
    createNewProduct: function() {
      var newProduct = {
        name: this.name,
        price: this.price,
        description: this.description
      };
      axios.post("/api/products", newProduct).then(response => {
        console.log(response.data);
      });
      this.name = "";
      this.price = "";
      this.description = "";
    },
  }
};
</script>