<template>
  <div class="home">
    <!-- <input type="text" v-model=""> -->
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