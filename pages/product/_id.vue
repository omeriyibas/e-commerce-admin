<template>
  <main>
    <div class="a-spacing-large"></div>

    <div class="container">
      <div class="row">
        <div class="col">
          <div class="card">
            <div class="col">
              <div class="d-flex d-xl-flex justify-content-xl-center align-items-xl-center"><img
                class="d-xl-flex justify-content-xl-center align-items-xl-end" style="width: 200px;"
                :src="product.image"/></div>
              <form>
                <div class="form-row">
                  <div class="col text-center">
                    <div class="form-group"><label class="text-center d-xl-flex justify-content-xl-center">Product Name:
                      {{ product.title }}</label></div>
                  </div>
                </div>
                <div class="form-row">
                  <div class="col">
                    <div class="form-group text-center"><label>Price: {{ product.price }}</label></div>
                  </div>
                  <div class="col">
                    <div class="form-group text-center"><label>Stock: {{ product.stockQuantity }}</label></div>
                  </div>
                </div>
                <div class="form-row">
                  <div class="col">
                    <div class="form-group text-center"><label>Brand:<img :src="product.brand.image" style="width: 50px;" ></label></div>
                  </div>

                  <div class="col">c
                    <div class="form-group text-center"><label>Category: {{ product.category.name }}</label></div>
                  </div>

                </div>
                <div class="form-row text-center">
                  <div class="col text-center">
                    <div class="form-group"><label class="text-center">descripton: {{ product.description }}</label></div>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="col">
          <div>
            <h1 class="text-center">Add New Product</h1>
            <form>
              <div class="form-row">
                <div class="col">
                  <div class="form-group"><label>Product Name</label><input class="form-control" type="text"
                                                                            v-model="title"/></div>
                </div>
              </div>
              <div class="form-row">
                <div class="col">
                  <div class="form-group"><label>Price</label><input class="form-control" type="number"
                                                                     v-model="price"/></div>
                </div>
                <div class="col">
                  <div class="form-group"><label>Stock</label><input class="form-control" type="number"
                                                                     v-model="stockQuantity"/></div>
                </div>
              </div>
              <div class="form-row">
                <div class="col">
                  <div class="form-group"><label>Category</label><select class="form-control" v-model="categoryID">
                    <option v-for="category in categories"
                            :value="category._id"
                            :key="category._id">{{ category.name }}
                    </option>
                  </select></div>
                </div>
                <div class="col">
                  <div class="form-group"><label>Brand</label><select class="form-control" v-model="brandID">
                    <option v-for="brand in brands"
                            :value="brand._id"
                            :key="brand._id">{{ brand.name }}
                    </option>
                  </select></div>
                </div>
              </div>
              <div class="form-row">
                <div class="col text-right">
                  <div class="form-group"><label class="text-right">Product
                    Description</label><textarea
                    class="form-control form-control-lg" v-model='description'></textarea></div>
                </div>
              </div>
              <div class="form-row">
                <div class="col">
                  <button class="btn btn-primary" type="button" @click="onUpdateProduct">Button</button>
                </div>
                <div class="col text-right">
                  <div class="form-group text-right"><input type="file" @change="onFileSelected"/></div>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>

export default {

  async asyncData({$axios, params}) {
    try {
      let categories = $axios.$get("http://localhost:3000/api/categories");
      let brands = $axios.$get("http://localhost:3000/api/brands");
      let product = $axios.$get(
        `http://localhost:3000/api/products/${params.id}`
      );

      const [catResponse, brandResponse, productResponse] = await Promise.all([
        categories,
        brands,
        product,
      ]);

      console.log(productResponse);

      return {
        categories: catResponse.categories,
        brands: brandResponse.brands,
        product: productResponse.product
      };

    } catch (err) {
      console.log(err);
    }
  },

  data() {
    return {
      categoryID: null,
      brandID: null,
      title: "",
      price: "",
      description: "",
      selectedFile: null,
      stockQuantity: "",
      fileName: "",
    };
  },

  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.fileName = event.target.files[0].name;
    },

    async onUpdateProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append('description', this.description);
      data.append("brandID", this.brandID);
      data.append("stockQuantity", this.stockQuantity);
      data.append("categoryID", this.categoryID);
      data.append("image", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$put(
        `http://localhost:3000/api/products/${this.$route.params.id}`,
        data
      );

      await this.$router.push("/");
    }
  }

}
</script>
