<template>
        <main>
                <div class="container-fluid">
                        <div class="row">
                                <div class="col-sm-3 a-spacing-bottom-medium" ></div>
                                <div class="col-sm-6">
                                        <div class="a-section">
                                                <div class="a-spacing-top-medium"></div>
                                                <h2 style="text-align: center">Update {{ product.title }}</h2>
                                                <form >
                                                        <!-- Category Dropdown -->
                                                <div class="a-spacing-top-medium">
                                                        <label for="">Category</label>
                                                        <select name="" id="" class="a-selection-option" v-model="categoryID">
                                                                <option
                                                                v-for="category in categories" 
                                                                :value="category._id" 
                                                                :key="category._id"
                                                                >{{ category.type }}</option>
                                                        </select>
                                                </div>

                                                        <!-- Owner Dropdown -->
                                                <div class="a-spacing-top-medium">
                                                        <label for="">Owner</label>
                                                        <select name="" id="" class="a-selection-option" v-model="ownerID">
                                                                <option 
                                                                v-for="owner in owners" 
                                                                :value="owner._id" 
                                                                :key="owner._id"
                                                                >{{ owner.name }}</option>
                                                        </select>
                                                </div>

                                                        <!-- Title Input -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Title</label>
                                                        <input type="text" class="a-input-text" style="width: 100%" v-model="title" :placeholder="product.title">
                                                </div>

                                                        <!-- Price Input -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Price</label>
                                                        <input type="number" class="a-input-text" style="width: 100%" v-model="price" :placeholder="product.price">
                                                </div>

                                                        <!-- Stock Quantity Input -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Stock Quantity</label>
                                                        <input type="number" class="a-input-text" style="width: 100%" v-model="stockQuantity" :placeholder="product.stockQuantity">
                                                </div>

                                                        <!-- Description textarea -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Description</label>
                                                        <textarea 
                                                        cols="10" 
                                                        rows="10" 
                                                        style="width: 100%" 
                                                        :placeholder="product.description"
                                                        v-model="description"></textarea>
                                                </div>

                                                        <!-- Photo file -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Add Photo</label>
                                                <div class="a-row a-spacing-top-medium">
                                                        <label class="choosefile-button">
                                                                <i class="fal fa-plus"></i>
                                                                        <input type="file" @change="onFileSelected">
                                                                        <p style="margin-top: -70px">{{ fileName }}</p>
                                                        </label>
                                                </div>
                                                </div>
                                                <!-- Button Dropdown --> 
                                                <hr/>
                                                <div class="a-spacing-top-large" style="margin-bottom: 100px">
                                                        <span class="a-button-register">
                                                                <span class="a-button-inner">
                                                                        <span class="a-button-text" @click="onAddProduct">Add product</span>
                                                                </span>
                                                        </span>
                                                </div>
                                                </form>
                                        </div>
                                </div>
                                <div class="col-sm-3"></div>
                        </div>
                </div>
        </main>
</template>

<script>
export default {
   async asyncData({ $axios, params }) {
    try{
      let categories = $axios.$get("http://localhost:2021/api/categories");
      let owners = $axios.$get("http://localhost:2021/api/owners");
      let product = $axios.$get(`http://localhost:2021/api/products/${params.id}`);
      
      const [ catResponse, ownerResponse, productResponse ] = await Promise.all([
              categories,
              owners,
              product
      ])
      console.log('pro 1',productResponse);
      console.log('pro',productResponse.product);

      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners,
        product: productResponse.product
      };
    }catch(error) {
      console.log(error);
    }
  },

  data() {
          return {
                  categoryID: null,
                  ownerID: null,
                  title: "",
                  price: "",
                  stockQuantity: "",
                  description: "",
                  selectedFile: null,
                  fileName: "",
                  sakamanje: 'Rolls Royce'
          }
  },

  methods: {
          onFileSelected(event) {
                  this.selectedFile = event.target.files[0];
                  console.log(this.selectedFile);
                  this.fileName = event.target.files[0].name;
          },
          async onAddProduct() {
                  try {
                          let data = new FormData();
                          data.append("title", this.title);
                          data.append("price", this.price);
                          data.append("description", this.description);
                          data.append("ownerID", this.ownerID);
                          data.append("cartegoryID", this.categoryID);
                          data.append("stockQuantity", this.stockQuantity);
                          data.append("photo", this.selectedFile, this.selectedFile.name);
                        //   console.log(FormData());
        
                          let result = await this.$axios.$put('http://localhost:2021/api/products', data)

                          this.$router.push("/")
                          
                  } catch (error) {
                          console.log(error);
                  }
          }
  }

}
</script>       