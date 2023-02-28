<template>
  <main>
    <div class="a-spacing-large"></div>

    <div class="container">
      <div class="row">
        <div class="col-xl-2" v-for="(product, index) in products"
             :key="product._id">
          <div class="card"><img class="card-img-top w-100 d-block" :src="product.image" style="width: 200px;">
            <div class="card-body">
              <h4 class="card-title">{{product.title}}</h4>
              <h6 class="card-title">price : {{product.price}}$</h6>
              <h6 class="card-title">category : {{product.category.name}}</h6>
              <h6 class="card-title">brand : <img :src="product.brand.image" width="40px"></h6>



              <div class="row">
                <div class="col-6">
                  <n-link :to="`/product/${product._id}`">
                  <button class="btn btn-primary btn-block" type="button">
                    <font-awesome-icon :icon="['far','edit']"></font-awesome-icon>
                  </button>
                  </n-link>
                </div>
                <div class="col-6">
                  <button class="btn btn-primary btn-block" type="button" @click="onDeleteProduct(product._id, index)">
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
      let response = await $axios.$get("http://localhost:3000/api/products");
      console.log(response);
      return {
        products: response.products
      };
    } catch (err) {
      console.log(err)
    }
  },
  methods: {
    async onDeleteProduct(id, index) {
      try {
        let response = await this.$axios.$delete(
          `http://localhost:3000/api/products/${id}`
        );

        if (response.status) {
          this.products.splice(index, 1); //görünen ürünleri azaltıyor
        }
      } catch (err) {
        console.log(err);
      }
    }
  }
}
</script>
