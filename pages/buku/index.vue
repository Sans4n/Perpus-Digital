<template>
  <div class="wrapper">
    <div class="content"></div>
    <div class="container-fluid">
      <h2 class="text-center my-4 text-white">RAK BUKU</h2>

      <!-- Pencarian -->
      <form @submit.prevent="getBooks" class="mb-4 d-flex justify-content-center">
        <div class="input-group rounded w-50">
          <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5"
            placeholder="Cari buku..." aria-label="Search" @input="handleInput" />
        </div>
      </form>

      <div class="text-white text-center mb-3">
        Menampilkan {{ books.length }} buku dari {{ books.length }}
      </div>

      <!-- Daftar Buku -->
      <div class="row justify-content-center">
        <div v-for="(book, i) in books" :key="i" class="col-md-3 col-sm-6 mb-4">
          <div class="card shadow-sm rounded">
            <nuxt-link :to="`/buku/${book.id}`">
              <div class="card-body text-center">
                <img :src="book.cover" class="cover img-fluid" alt="cover">
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>

      <!-- Tombol Menu -->
      <div class="row d-flex justify-content-center mt-4">
        <nuxt-link to="/pengunjung/menu" class="btn-custom">Menu</nuxt-link>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useSupabaseClient } from "#imports"; // Ganti importnya

const supabase = useSupabaseClient();
const keyword = ref("");
const books = ref([]);

const getBooks = async () => {
  const { data } = await supabase
    .from("buku")
    .select("*, kategori(*)")
    .ilike("judul", `%${keyword.value}%`)
    .order("id");

  if (data) books.value = data;
};

onMounted(() => {
  getBooks();
});
</script>


<style scoped>
/* Background */
.content {
  background-image: url("@/assets/img/bg-home-cari-buku.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  position: fixed;
  top: 0;
  background-position: center;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: -1;
}

/* Kartu Buku */
.card {
  width: 100%;
  height: auto;
  padding: 0;
}

.cover {
  width: 100%;
  height: 300px;
  object-fit: cover;
  object-position: center;
}

/* Tombol */
.btn-custom {
  background-color: white;
  color: black;
  width: 160px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  font-weight: bold;
  border-radius: 10px;
  text-decoration: none;
}
</style>
