<template>
   <main>
     <div class="a-spacing-large"></div>
     
     <div class="container-fluid browsing-history">
       <div class="row">
         <div class="col-sm-8 col-8">
           <h1 class="a-size-large a-spacing-none a-text-normal">All products </h1>
            <div class="a-spacing-large"></div>
            <!-- <Button></Button> -->
           <nuxt-link to="/products" class="a-button-buy-again">Add a new product</nuxt-link>
           <nuxt-link to="/category" class="a-button-history margin-right-10">Add a new category</nuxt-link>
           <nuxt-link to="/owner" class="a-button-history margin-right-10   ">Add a new owner</nuxt-link>
           <!-- Listing PAge  -->
         </div> 
       </div>
     </div>
     <div class="a-spacing-large"></div>


     <div class="container-fluid browsing-history">
       <div class="row">
         <div v-for="(product, index) in products" :key="product._id" class="col-xl-2 col-lg-2 col-md-3 col-sm-6 br bb">
         <div class="history-box">
           <!-- Product image -->
           <a href="#" class="a-link-normal">
             <img :src="product.photo" alt="" class="img-fluid">
           </a>
           <!-- product title -->
           <div class="a-spacing-top-base asin-title">
             <span class="a-text-normal">
               <div class="p13n-sc-truncated">{{ product.title }}</div>
             </span>
           </div>
           <!-- prduct rating -->
           <div class="a-row">
             <a href="#">
               <i class="fas fa-star"></i>
               <i class="fas fa-star"></i>
               <i class="fas fa-star"></i>
               <i class="fas fa-star"></i>
               <i class="fas fa-star"></i>
             </a>
             <span class="a-letter-space"></span>
             <span class="a-color-tertiary a-size-small asin-reviews">(1342)</span>
           </div>
           <!-- product price -->
           <div class="a-row">
             <span class="a-size-base a-color-price">
               <span class="p13n-sc-price">{{ product.price }}</span>
             </span>
           </div>
           <!-- product buttons -->
           <div class="a-row">
             <nuxt-link :to="`/products/${product._id}`" class="a-button-history margin-right-10" >Update</nuxt-link>
             <a href="#" class="a-button-history margin-right-10" @click="onDeleteProduct(product._id)">Delete</a>
           </div>
           </div>
           </div>         
       </div>
     </div>
   </main>
</template>

<script>
export default {

  // asyncData is fetching data before nuxt app finish loading on the browser.
  // It is good for SEO because the data will be loaded first.
  async asyncData({ $axios }) {
    try{
      let response = await $axios.$get("http://localhost:2021/api/products");
      console.log(response);
      return {
        products: response.products
      };
    }catch(error) {
      console.log(error);
    }
  },

  methods: {
    async onDeleteProduct(id, index) {
         try {
              let response = await this.$axios.$delete(`http://localhost:2021/api/products/${id}`);

              if (response.status) {
                this.products.splice(index, 1)
              }
         } catch (error) {
            console.log(error);
        }
    }
  }
}
</script>

<style>

</style>
