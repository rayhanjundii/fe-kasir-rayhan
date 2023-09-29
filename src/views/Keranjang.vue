<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Pesanan
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <h2>Tambah Pensanan Makanan</h2>

      <div class="row">
        <div class="col-md-8">
          <div class="row">
            <div
              class="col-md-4 mt-4"
              v-for="product in products"
              :key="product.id"
            >
              <div
                class="card shadow card-product"
                @click="addToCartHandler(product)"
              >
                <img
                  :src="getProductImageUrl(product.gambar)"
                  class="card-img-top"
                  alt="..."
                />
                <div class="card-body">
                  <h5 class="card-title">{{ product.nama }}</h5>
                  <p class="card-text">Harga : Rp. {{ product.harga }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-4">
          <div class="card">
            <h3 class="text-center">Pesanan</h3>
            <div
              v-for="item in pesanan"
              :key="item.id"
              class="d-flex justify-content-between align-items-center"
            >
              <span
                >{{ item.nama }}
                <span v-if="item.quantity > 1">x{{ item.quantity }}</span></span
              >
              <span>Rp. {{ item.harga * item.quantity }}</span>
              <button
                @click="removeFromCart(item)"
                class="btn btn-sm btn-danger"
              >
                Hapus
              </button>
            </div>

            <div class="text-center mt-3">
              <strong>Total: Rp. {{ getTotal() }}</strong>
            </div>
            <div class="container mb-2">
              <div class="row justify-content-center">
                <div class="col-md-12 text-center mb-2">
                  <button
                    @click="clearCart"
                    class="btn btn-outline-danger col-md-12"
                  >
                    Bersihkan Keranjang
                  </button>
                </div>
                <div class="col-md-6 text-center mb-2">
                  <button @click="saveBill" class="btn btn-success col-md-12">
                    Simpan Bill
                  </button>
                </div>
                <div class="col-md-6 text-center mb-2">
                  <button @click="printBill" class="btn btn-success col-md-12">
                    Cetak Bill
                  </button>
                </div>
                <div class="col-md-12 text-center">
                  <button @click="charge" class="btn btn-primary col-md-12">
                    Charge
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";
import Swal from "sweetalert2";
import { baseUrl } from "../config";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      products: [],
      pesanan: [
        [
          {
            id: 12,
            nama: "Jagung Manis",
            harga: 100000,
            quantity: 1,
          },
        ],
      ],
    };
  },

  methods: {
    getProductImageUrl(gambar) {
      return baseUrl + gambar;
    },
    setProducts(data) {
      this.products = data;
    },
    addToCartHandler(product) {
      this.addToCart(product);
      console.log("Produk ditambahkan ke dalam keranjang:", this.pesanan);
    },

    addToCart(pesanan) {
      const existingProductIndex = this.pesanan.findIndex(
        (item) => item.id === product.id
      );
      if (existingProductIndex !== -1) {
        this.pesanan[existingProductIndex].quantity++;
        // console.log(this.pesanan)
      } else {
        this.pesanan.push({
          id: pesanan.id,
          nama: pesanan.nama,
          harga: pesanan.harga,
          quantity: 1,
        });
        // console.log(this.pesanan)
      }
    },
    removeFromCart(product) {
      const index = this.products.findIndex((item) => item.id === product.id);
      if (index !== -1) {
        this.products[index].quantity--;
        if (this.products[index].quantity <= 0) {
          this.products.splice(index, 1);
        }
      }
    },

    clearCart() {
      // Hapus semua produk dari keranjang
      this.products = [];
    },
    getTotal() {
      // Menghitung total harga belanjaan di keranjang
      return this.products.reduce(
        (total, item) => total + item.harga * item.quantity,
        0
      );
    },
    saveBill() {
      Swal.fire({
        icon: "success",
        title: "Bill Disimpan!",
        text: "Bill telah disimpan.",
      });
    },
    charge() {
      Swal.fire({
        title: "Total Charge",
        text: "Masukkan jumlah uang pembeli:",
        input: "number",
        showCancelButton: true,
        confirmButtonText: "OK",
        preConfirm: (amount) => {
          // Menghitung kembalian
          const change = amount - this.getTotal();
          if (change >= 0) {
            Swal.fire({
              icon: "success",
              title: "Transaksi Berhasil!",
              text: `Kembalian: Rp. ${change}`,
            });
          } else {
            Swal.fire({
              icon: "error",
              title: "Pembayaran Kurang!",
              text: "Uang pembeli tidak mencukupi.",
            });
          }
        },
      });
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
