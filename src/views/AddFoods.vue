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
                Tambah Makanan
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <h2>Tambah Makanan</h2>
      <div class="row">
        <div class="col-md-6">
          <form @submit.prevent="addFood">
            <div class="form-group">
              <label for="nama">Nama Makanan</label>
              <input
                type="text"
                id="nama"
                v-model="newFood.nama"
                class="form-control"
                required
              />
            </div>
            <div class="form-group">
              <label for="gambar">Gambar Makanan</label>
              <input
                type="file"
                id="gambar"
                @change="handleFileUpload"
                class="form-control-file"
              />
            </div>
            <div class="form-group">
              <label for="harga">Harga Makanan</label>
              <input
                type="number"
                id="harga"
                v-model="newFood.harga"
                class="form-control"
                required
              />
            </div>
            <button type="submit" class="btn btn-primary">Simpan</button>
          </form>
        </div>
        <div class="col-md-6">
          <div class="col-md-6">
            <div class="form-group">
              <label for="preview">Preview Gambar</label>
              <div id="preview">
                <img
                  :src="previewImage"
                  alt="Preview"
                  class="img-fluid"
                  v-if="previewImage"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import axios from "axios"; 
export default {
  components: {
    Navbar,
  },
  data() {
    return {
      newFood: {
        nama: "",
        gambar: "", // Untuk menyimpan data gambar yang akan diunggah
        harga: "",
      },
      previewImage: null, // Untuk menampilkan gambar yang akan diunggah (preview)
    };
  },
  methods: {
    addFood() {
      // Membuat objek FormData untuk mengirim file gambar
      const formData = new FormData();
      formData.append("nama", this.newFood.nama);
      formData.append("gambar", this.newFood.gambar);
      formData.append("harga", this.newFood.harga);
      console.log(this.newFood);
      // Mengirim data ke server menggunakan Axios
      axios
        .post("http://localhost:8000/api/makanan", formData)
        .then((response) => {
          console.log("Data berhasil dikirim:", response.data);
          this.$router.push("/");
        })
        .catch((error) => {
          console.error("Terjadi kesalahan:", error);
          // Tambahkan tindakan untuk menangani kesalahan jika diperlukan
        });
    },
    handleFileUpload(event) {
      // Mengambil data gambar yang diunggah oleh pengguna
      const file = event.target.files[0];
      // console.log(file.name);
      if (file) {
        this.newFood.gambar = file;
        // Tampilkan gambar yang akan diunggah (preview)
        const reader = new FileReader();
        reader.onload = (e) => {
          this.previewImage = e.target.result;
        };
        reader.readAsDataURL(file);
      }
    },
  },
};
</script>
