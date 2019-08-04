<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="name"></p>
    <p>Price: <input type="text" v-model="price"></p>
    <p>Description: <input type="text" v-model="description"></p>
    <button v-on:click="createNewProduct">Create a new Product</button>
    <div v-for="product in products">
      <p>{{product.name}}</p>
      <p>{{product.price}}</p>
      <img v-bind:src="product.image_url" alt="product.name" width="300px">
      <p><button v-on:click="changeCurrentProduct(product)">Show More Info</button></p>
      <div v-if="currentProduct === product">
        <p>{{product.description}}</p>
        <p>Name: <input type="text" v-model="product.name"></p>
        <p>Price: <input type="number" v-model="product.price"></p>
        <p>Description: <input type="text" v-model="product.description"></p>
        <button v-on:click="updateProduct(product)">Update Product</button>
      <br>
      <p><button v-on:click="deleteProduct(product)">Delete Product</button></p>
      </div>
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      name: "",
      price: "",
      description: "",
      currentProduct: ""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {
    createNewProduct: function() {
      var newProduct = {
        name: this.name,
        price: this.price,
        description: this.description
      };
      axios.post("/api/products", newProduct).then(response => {
        console.log(response.data);
        this.products.push(response.data);
      });
      this.name = "";
      this.price = "";
      this.description = "";
    },
    changeCurrentProduct: function(theProduct) {
      this.currentProduct = theProduct;
    },
    updateProduct: function(theProduct) {
      axios.patch("/api/products/" + theProduct.id, theProduct).then(response => {
        console.log(response.data);
        theProduct.name = response.data.name;
        theProduct.price = response.data.price;
        theProduct.description = response.data.description;
      });
    },
    deleteProduct: function(theProduct) {
      console.log("in deleteProduct");
      axios.delete("/api/products/" + theProduct.id).then(response => {
        console.log(response.data);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>