<template>
  <main>
    <div class="a-spacing-large"></div>

    <div class="container">
      <form>
        <div class="form-row">
          <div class="col">
            <div class="form-group"><label>Brand Name</label>
              <div class="form-row">
                <div class="col-10"><input class="form-control" type="text" v-model="name"></div>
                <div class="col-auto">
                  <button class="btn btn-primary" type="button" @click="onAddBrand">
                    <font-awesome-icon :icon="['fas','plus-square']"></font-awesome-icon>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="form-row text-right">
          <div class="col-3 text-left"><input type="file" @change="onFileSelected"></div>
          <div class="col-7"><select class="form-control" v-model="categoryID">
            <option v-for="category in categories" :value="category._id" :key="category._id">{{category.name}}</option>
          </select>
        </div>
        </div>
      </form>
      <div class="a-spacing-large"></div>
      <div class="row justify-content-center features">
        <div class="col-xl-2" v-for="brand in brands" :key="brand._id">
          <div class="card"><img class="card-img-top w-100 d-block" :src="brand.image" style="width: 200px;">
            <div class="card-body">
              <h4 class="card-title">{{ brand.name }}</h4>
              <h6 class="card-title">{{ brand.category.name}}</h6>

              <div class="row">
                <div class="col text-center">
                  <button class="btn btn-primary text-center" type="button" style="text-align: center;"
                          @click="onDeleteBrand(brand._id,index)">
                    <font-awesome-icon :icon="['far','trash-alt']"></font-awesome-icon>
                  </button>
                </div>
              </div>
            </div>
          </div>
          <div class="a-spacing-large"></div>

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

  data() {
    return {
      name: "",
      categoryID:"",
      selectedFile: null,
      fileName: ""
    };
  },

  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.fileName = event.target.files[0].name;
    },
    async onAddBrand() {
      try {
        let data = new FormData();
        if(this.name!==null)data.append("name", this.name);
        if(this.selectedFile!==null)data.append("image", this.selectedFile, this.selectedFile.name);
        if(this.categoryID!==null)data.append("categoryID", this.categoryID);
        let response = await this.$axios.$post(
          "http://localhost:3000/api/brands",
          data
        );
      } catch (err) {
        console.log(err);
      }
      await this.$nuxt.refresh()
    },
    async onDeleteBrand(id, index) {
      try {
        let response = await this.$axios.$delete(
          `http://localhost:3000/api/brands/${id}`
        );

        if (response.status) {
          this.brands.splice(index, 1); //görünen kategori azaltıyor
        }
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>
