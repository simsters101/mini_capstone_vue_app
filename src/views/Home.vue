<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <p>{{product.name}}</p>
      <p>${{product.price}}0</p>
      <img v-bind:src="product.image_url" alt="product.name" width="300px">
      <p><button v-on:click="redirectToShow(product)">Show More Info</button></p>
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
      message: "Welcome to the Smartphone Store!",
      products: [],
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
    redirectToShow: function(theProduct) {
      this.$router.push("/products/" + theProduct.id);
    },
    updateProduct: function(theProduct) {
      axios.patch("/api/products/" + theProduct.id, theProduct).then(response => {
        console.log(response.data);
        theProduct.name = response.data.name;
        theProduct.price = response.data.price;
        theProduct.description = response.data.description;
      });
    }
  }
};
</script>