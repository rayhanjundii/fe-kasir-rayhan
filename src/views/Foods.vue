<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>
            Daftar
            <strong>Makanan</strong>
          </h2>
        </div>
      </div>
      <div class="row mb-4">
        <div class="container mb-3">
          <router-link class="btn btn-info" to="/add-foods">Tambah Makanan</router-link>
        </div>
      </div>
      <div class="row mb-4">
          <div class="container mb-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Nama Makanan</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Harga</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(product, index) in products" :key="product.id">
                  <th>{{ index + 1 }}</th>
                  <td>{{ product.nama }}</td>
                  <td><img :src="getProductImageUrl(product.gambar)" alt="Gambar Produk" width="100" height="100"></td>
                  <td>Rp. {{ product.harga }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import { baseUrl } from '@/config.js';

import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
  },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    searchFood() {
      axios
        .get("http://localhost:8000/api/makanan?q=" + this.search)
        .then((response) => this.setProducts(response.data))
        .catch((error) => console.log(error));
    },
    getProductImageUrl(gambar) {
      return baseUrl + gambar;
    },
  },
  mounted() {
    axios
      .get("http://localhost:8000/api/makanan")
      .then((response) => this.setProducts(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
<style></style>
