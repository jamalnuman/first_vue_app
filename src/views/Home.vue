<template>
  <div class="home">
    <h1>{{message}}</h1>
    <div>
      <h2>Add Product:</h2>
      <input type="text" placeholder="product name" v-model="productName"> 
      <input type="text" placeholder="product price" v-model="productPrice">
      <input type="text" placeholder="product description" v-model="productDescription">
      <input type="text" placeholder="product image_url" v-model="productImageUrl">
      <button v-on:click="AddRecipe()"> Add Recipe! </button>
    </div>
    
    <div v-for="product in products">
      <img v-on:click="currentProduct = product" v-bind:src="product.image_url">
      <p><strong>Product ID:</strong> {{product.id}}</p>
      <p> <strong>Product Name:</strong> {{ product.name }}</p>
      <p><strong>Product Price:</strong> {{ product.price }}</p>
      <div v-if="product === currentProduct">
        <p><strong>Product Description:</strong> {{ product.description }}</p>  
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
    }
    // DeleteRecipe: function(){

      //   let params = { id: this.product_id }

      //   console.log(params)
        

      // axios.delete("/api/products/", params["id"]).then(response => {
      //   console.log('Product has been destroyed!')
      // });
    // },
  }
};
</script>

