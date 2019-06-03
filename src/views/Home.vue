<template>
  <div class="home">
    <h1> New Product </h1>

    <div>
      Name: <input v-model="newProductName">
    </div>

    <div>
      Description: <input v-model="newProductDescription">
    </div>

    <div>
      Price: <input v-model="newProductPrice">
    </div>

    <div>
      Image: <input v-model="newProductImageUrl">
    </div>

    <button v-on:click="createProduct()"> Create </button>

    <h1> All Products </h1>

    <div v-for="product in products">
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
      <h1> Name: {{product.name }}</h1>

      <div v-if="currentProduct === product">
        <h2> Description: {{product.description}}</h2>
        <h2> Price: {{product.price}}</h2>
      </div>

      <button v-on:click="showProduct(product)"> More Info</button>
      <h3> Edit Product </h3>
      <div>
        <div>
          Name: <input v-model="product.name">
        </div>

        <div>
          Description: <input v-model="product.description">
        </div>

        <div>
          Price: <input v-model="product.price">
        </div>

        <div>
          Image: <input v-model="product.image_url">
        </div>

        <button v-on:click="updateProduct(product)">Update</button>
      </div>
    </div>
    
  </div>
</template>


<style>
  img {
        width: 250px;
      }
</style>

<script>
  var axios = require('axios');
export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
      currentProduct: {}
    };
  },
  created: function() {

  axios.get("/api/products").then(response => {
    this.products = response.data;
  });
  },
  methods: {
    createProduct: function(){
      console.log("Create the product");

      var params = {
                    name: this.newProductName,
                    description: this.newProductDescription,
                    price: this.newProductPrice,
                    image_url: this.newProductImageUrl
      };
      axios.post("/api/products", params).then(response => {
        console.log("Success", response.data);
        this.products.push(response.data);

        this.newProductName = "";
        this.newProductDescription = "";
        this.newProductPrice= "";
        this.newProductImageUrl= "";
      });
      
    },
    showProduct: function(inputProduct){
      if (this.currentProduct === inputProduct){
        this.currentProduct = {};  
      } else {
      this.currentProduct = inputProduct;
      }
    },
    updateProduct: function(inputProduct){
       var params = {
                    name: inputProduct.name,
                    description: inputProduct.description,
                    price: inputProduct.price,
                    image_url: inputProduct.image_url
                    };
      axios.patch("api/products/" + inputProduct.id, params).then(response => {
        console.log("Success", response.data);
      });
    }
  }
};
</script>
