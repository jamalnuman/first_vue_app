<template>
  <div class="home">
    <h1>{{message}}</h1>
    <div class="new-form">
      <h2>Add Product:</h2>
      <input type="text" placeholder="product name" v-model="productName"> 
      <input type="text" placeholder="product price" v-model="productPrice">
      <input type="text" placeholder="product description" v-model="productDescription">
      <input type="text" placeholder="product image_url" v-model="productImageUrl">
      <button v-on:click="AddRecipe()"> Add Recipe! </button>
    </div> 
    
    <div v-for="product in products">
      <img v-on:click="showProducts(product)" v-bind:src="product.image_url">
      <p><strong>Product ID:</strong> {{product.id}}</p>
      <p> <strong>Product Name:</strong> {{ product.name }}</p>
      <p><strong>Product Price:</strong> {{ product.price }}</p>
      <div class="show-page" v-if="product === currentProduct">
        <p><strong>Product Description:</strong> {{ product.description }}</p> 
        <div class="edit-form">
          <h4>Edit Product</h4>
          <div>
            Name: <input type="text" v-model="product.name">
          </div>
          <div>
            Price: <input type="text" v-model="product.price">
          </div>
          <div>
            Description: <input type="text" v-model="product.description">
          </div>
          <div>
            Image_Url: <input type="text" v-model="product.image_url">
          </div>
          <button v-on:click="updateProduct(product)">Update</button>
        </div> 
        <div class="destroy-action">
          <button v-on:click="destroyProduct(product)">Destroy Product!</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

</style>

<script>
  let axios = require('axios');//this is the safe method of 'requiring' something in the browser..as seen in the node video...on monday
export default { //the Vue object lives within the scope of the export default hash. The methods above are connected to the data hash because of the v-model directive.
  data: function() {
    return {
      currentProduct: {},
      message: "Welcome to Vue.js!",
      products: [],
      productName: "",
      productPrice: "",
      productDescription: "",
      productImageUrl: ""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      // console.log(response.data)
      this.products = response.data;
    });
  },
  methods: {
    AddRecipe: function(){
      console.log("sumbitting...")

      let params = {
        name: this.productName, 
        price: this.productPrice,
        description: this.productDescription,
        image_url: this.productImageUrl 
      };

      // console.log(params)

      axios.post("/api/products", params).then(response => {
        console.log(response.data);
        //this.response1.push(response.data);
      });
    },

    showProducts: function(input){
      if (this.currentProduct !== input){
        this.currentProduct = input;
      } else {
        this.currentProduct = {};
      }
    },

    updateProduct: function(input){
      let clientParams = {
        name: input.name,
        price: input.price,
        description: input.description,
        image_url: input.image_url
      };

      axios
      .patch("api/products/" + input.id, clientParams)
      .then(response => {
        console.log("Success")
      }).catch(error => {
        console.log(error.response.date);
      });
    },

    destroyProduct: function(input) {
      axios
      .delete("/api/products/" + input.id)
      .then(response =>{
        console.log("Product Destroyed!");
        var index = this.products.indexOf(input);
        this.products.splice(index, 1);
      });
    },
  }
};
</script>

