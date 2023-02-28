<template>
  <main>
    <div class="a-spacing-large"></div>

    <div class="container">
      <div class="row">
        <div class="col">
          <form>
            <div class="col">
              <div class="form-group"><label>Category Name</label>
                <div class="form-row">
                  <div class="col-10"><input type="text" class="form-control" v-model="name"/></div>
                  <div class="col-auto">
                    <button class="btn btn-primary" type="button" @click="onAddCategory">
                      <font-awesome-icon :icon="['fas','plus-square']"></font-awesome-icon>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
      </div>
      <hr/>
      <div class="row justify-content-center features" v-for="category in categories"
           :key="category._id">
        <div class="col-6">
          <div class="card">
            <div class="card-body">
              <div class="row">
                <div class="col"><label class="col-form-label">{{ category.name }}</label></div>
                <div class="col-2">
                  <button class="btn btn-primary" type="button" @click="onDeleteCategory(category._id,index)">
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
  async asyncData({$axios}) {
    try {
      let response = await $axios.$get("http://localhost:3000/api/categories");
      return {
        categories: response.categories
      };
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      name: ""
    }
  },

  methods: {
    async onAddCategory() {
      try {
        let data = {name: this.name};
        let response = await this.$axios.$post(
          "http://localhost:3000/api/categories",
          data
        );
        if (response.status) {
          this.categories.push(data)
        }
      } catch (err) {
        console.log(err)
      }
      await this.$nuxt.refresh()

    },
    async onDeleteCategory(id, index) {
      try {
        let response = await this.$axios.$delete(
          `http://localhost:3000/api/categories/${id}`
        );

        if (response.status) {
          this.categories.splice(index, 1); //görünen kategori azaltıyor
        }
      } catch (err) {
        console.log(err);
      }
    }


  }
}
</script>
