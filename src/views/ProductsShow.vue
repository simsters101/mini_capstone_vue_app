<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>{{ product.name }}</p>
    <p><img v-bind:src="product.image_url" alt="product.name" width="300px"></p>
    <p>${{ product.price }}0</p>
    <p>{{ product.description }}</p>
    <p><button v-on:click="redirectToEdit(product)">Edit Product</button></p>
    <p><button v-on:click="deleteProduct(product)">Delete Product</button></p>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to The Show!",
      product: []
    };
  },
  created: function() {
    console.log(this.$route.params.id);
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      console.log(response.data);
      this.product = response.data;
    });
  },
  methods: {
    deleteProduct: function(theProduct) {
      console.log("in deleteProduct");
      axios.delete("/api/products/" + theProduct.id).then(response => {
        console.log(response.data);
        this.$router.push("/");
      });
    },
    redirectToEdit: function(theProduct) {
      this.$router.push("/products/edit/" + theProduct.id);
    }
  }
};
</script>