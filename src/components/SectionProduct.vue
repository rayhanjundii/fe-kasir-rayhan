<template>
  <div class="col-md-7">
    <div class="row mb-4">
        <div class="col-md-4 mt-4" v-for="product in products" :key="product.id">
          <CardProduct :product="product" />
        </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CardProduct from "./CardProduct.vue";

export default {
  name: "SectionProduct",
  components: {
    CardProduct,
  },
  data() {
    return {
      product: [],
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
  },
  mounted() {
    axios
    .get("http://localhost:8000/api/makanan")
    .then((response)=>this.setProduct(response.data))
    .catch((error)=>console.log(error))
  },
};
</script>
