<template>
        <main>
                <div class="container-fluid">
                        <div class="row">
                                <div class="col-sm-3 a-spacing-bottom-medium" ></div>
                                <div class="col-sm-6">
                                        <div class="a-section">
                                                <div class="a-spacing-top-medium"></div>
                                                <h2 style="text-align: center">Add a new product</h2>
                                                <form >
                                                        <!-- Category Dropdown -->
                                                <div class="a-spacing-top-medium">
                                                        <label for="">Category</label>
                                                        <select name="" id="" class="a-selection-option">
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
                                                        <select name="" id="" class="a-selection-option">
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
                                                        <input type="text" class="a-input-text" style="width: 100%">
                                                </div>

                                                        <!-- Price Input -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Price</label>
                                                        <input type="number" class="a-input-text" style="width: 100%">
                                                </div>

                                                        <!-- Description textarea -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Description</label>
                                                        <textarea name="" id="" cols="30" rows="10" style="width: 100%" placeholder="Provide details such as a product decsription"></textarea>
                                                </div>

                                                        <!-- Photo file -->
                                                <div class="a-spacing-top-medium">
                                                        <label style="margin-bottom: 0px;">Add Photo</label>
                                                <div class="a-row a-spacing-top-medium">
                                                        <label class="choosefile-button">
                                                                <i class="fal fa-plus"></i>
                                                                        <input type="file">
                                                                        <p style="margin-top: -70px">Name of thes</p>
                                                        </label>
                                                </div>
                                                </div>
                                                <!-- Photo Dropdown -->
                                                <hr/>
                                                <div class="a-spacing-top-large">
                                                        <span class="a-button-register">
                                                                <span class="a-button-inner">
                                                                        <span class="a-button-text">Add product</span>
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
   async asyncData({ $axios }) {
    try{
      let categories = $axios.$get("http://localhost:2021/api/categories");
      let owners = $axios.$get("http://localhost:2021/api/owners");
      
      const [ catResponse, ownerResponse ] = await Promise.all([
              categories,
              owners
      ])
      console.log(catResponse);

      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners
      };
    }catch(error) {
      console.log(error);
    }
  }
}
</script>