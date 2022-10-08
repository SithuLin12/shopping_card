<template>
  <div>
    <div class=" row bg-secondary text-dark ">
      <div class="col-6 p-3">
        <button @click="productPageBtn()" class="btn btn-success">Product view</button>
      </div>
      <div class="col-5 p-3 ">
        <button type="button" @click="cartPageBtn()" class="btn btn-primary position-relative">
          Cart
          <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
           {{ cart.length }}
            <span class="visually-hidden">unread messages</span>
          </span>
        </button>
      </div>
    </div>
    <div class="" v-if="productPage">
      <h1>Products</h1>
    
    <h4 class="text-center" v-if="loadStatus">loading.....</h4>

    <div class="row"  v-else>
      <div class="col" v-for="(product,index) in products" :key="index">
        <div class="card">
        <div class="card-body">
          <img :src="product.image" class="card-image" alt="" style="height:200px">

          <hr>
          <h3>{{ product.title }}</h3>
      
        <div class="">{{ product.price }}</div>
        <button @click="addItemToCart(product)" class="btn btn-dark">Add to cart</button>
        </div>
      </div>
      </div>
    </div>
    </div>

    <div class="" v-else>
      <h2>This is cart Page</h2>
      <div class="">
        <table class="table table-striped table-hover">
          <tr>
            <th>Image</th>
            <th>Name</th>
            <th>Price</th>
            <th></th>
          </tr>

          <tr v-for="(c,index) in cart" :key="index">
            <td><img :src="c.image" style="height:100px;"></td>
            <td>{{ c.title }}</td>
            <td>{{ c.price}}</td>

            <td><button  @click="removeBtn(index)" class="btn bg-danger text-white">remove</button></td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name : "App",
    data() {
      return {
        productPage : true,
        loadStatus : false,
        cart: [],
        nonCart: { name : "", price : "", image : ""},
        products: [
        ]
      }
    },

    methods: {
      addItemToCart(product) {
        this.cart.push(product)
        localStorage.setItem('myCart',JSON.stringify(this.cart))
      },
      cartPageBtn(){
        this.productPage = false
      },
      productPageBtn(){
        this.productPage = true
      },
      removeBtn(id){
           this.cart.splice(id,1)
           localStorage.setItem('myCart',JSON.stringify(this.cart))
      }
    },

    mounted () {
      this.loadStatus = true     
      axios.get("https://fakestoreapi.com/products").then((response) => {
        this.loadStatus = false
        this.products = response.data
      }).catch((err) => {
        console.log(err);
      });

      let data = localStorage.getItem("myCart")
      if(data != null ){
        this.cart = JSON.parse(data);
      }
    },
  }
</script>