<template>
  <main>
    <div class="a-spacing-large"></div>

    <div class="container">
      <div class="row row-cols-2 text-left m-5">
        <div class="col">
          <form>
            <div class="form-group"><label>County Name:</label><input type="text" class="form-control" v-model="name" /></div>
          </form>
        </div>
        <div class="col text-left">
          <form>
            <div class="form-group"><label>Shipping Price: </label><input type="text" class="form-control" v-model="shippingPrice" /></div>
          </form>
        </div>
        <div class="col"></div>
        <div class="col text-right">
        <button class="btn btn-primary text-right" type="button" @click="onAddCounty">
          Add
          <font-awesome-icon :icon="['fas','plus-square']"></font-awesome-icon>
        </button>
      </div>
      </div>
      <hr/>
      <div class="row justify-content-center features" v-for="county in counties"
           :key="county._id">
        <div class="col-6">
          <div class="card">
            <div class="card-body">
              <div class="row">
                <div class="col"><label class="col-form-label">{{ county.name }}  {{county.shipPrice}}</label></div>
                <div class="col-2">
                  <button class="btn btn-primary" type="button" @click="onDeleteCounty(county._id,index)">
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
      let response = await $axios.$get("http://localhost:3000/api/counties");
      return {
        counties: response.counties
      };
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      name: "",
      shippingPrice:"",
    }
  },
  methods:{
    async onAddCounty(){
      try {
        let data = {name: this.name,shipPrice:this.shippingPrice};
        let response = await this.$axios.$post(
          "http://localhost:3000/api/county",
          data
        );
        if (response.status) {
          this.counties.push(data)
        }
      } catch (err) {
        console.log(err)
      }
      await this.$nuxt.refresh()

    },
    async onDeleteCounty(id,index){
      try {
        let response = await this.$axios.$delete(
          `http://localhost:3000/api/county/${id}`
        );

        if (response.status) {
          this.counties.splice(index, 1); //görünen kategori azaltıyor
        }
      } catch (err) {
        console.log(err);
      }

    }
  }
}
</script>

<style>

</style>
