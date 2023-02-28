<template>
  <main>
    <div class="a-spacing-large"></div>

    <div class="container">
      <div class="row">
        <div class="col">
          <div>
            <form>
              <div class="form-row">
                <div class="col">
                  <div class="form-group"><label>Product Name</label><input class="form-control" type="text"
                                                                            v-model="title"></div>
                </div>
              </div>
              <div class="form-row">
                <div class="col">
                  <div class="form-group"><label>Price</label><input class="form-control" type="number" v-model="price">
                  </div>
                </div>
                <div class="col">
                  <div class="form-group"><label>Stock</label><input class="form-control" type="number"
                                                                     v-model="stockQuantity"></div>
                </div>
              </div>
              <div class="form-row">
                <div class="col">
                  <div class="form-group"><label>Category</label><select class="form-control" v-model="categoryID">
                    <option
                      v-for="category in categories"
                      :value="category._id"
                      :key="category._id"> {{ category.name }}
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
                  <div class="form-group"><label class="text-right">Product Description</label><textarea
                    class="form-control form-control-lg" v-model="description"></textarea></div>
                </div>
              </div>
              <div class="form-row">
                <div class="col">
                  <button class="btn btn-primary" type="button" @click="onAddProduct"><font-awesome-icon :icon="['fas','plus-square']"/> Add</button>
                </div>
                <div class="col text-right">
                  <div class="form-group text-right"><input type="file" @change="onFileSelected"></div>
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

  async asyncData({$axios}) { //serverla iletişin
    try {
      let categories = await $axios.$get("http://localhost:3000/api/categories");
      let brands = await $axios.$get("http://localhost:3000/api/brands");

      const [catResponse, brandResponse] = await Promise.all([ //hepsnin yüklenmesini bekle, değeri al
        categories, brands
      ])

      console.log(catResponse)

      return {
        categories: catResponse.categories,
        brands: brandResponse.brands
      };
    } catch (err) {
      console.log(err)
    }
  },
  data() { //client ile iletişim
    return {
      categoryID: null,
      brandID: null,
      title: null,
      price: null,
      description: "",
      selectedFile: null,
      stockQuantity: null,
      fileName: "",
    }
  },

  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0] //seçelen dosya objesini ata
      this.fileName = event.target.files[0].name
    },
    async onAddProduct() {
      let data = new FormData() //hepsini form objesi haline getirdik
      if(this.title) data.append('title', this.title)
      if(this.price)data.append('price', this.price)
      if(this.description)data.append('description', this.description)
      if(this.brandID)data.append('brandID', this.brandID)
      if(this.stockQuantity)data.append('stockQuantity', this.stockQuantity)
      if(this.categoryID)data.append('categoryID', this.categoryID)
      if(this.selectedFile)data.append('image', this.selectedFile, this.selectedFile.name)

      let result = await this.$axios.$post('http://localhost:3000/api/products', data) //axios objeyi alıp post edicek

      await this.$router.push("/")

    }
  }
}
</script>
