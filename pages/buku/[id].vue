<template>
  <div class="content">
    <div class="container-fluid pt-5">
      <h2 class="text-center text-white mb-5">RINCIAN BUKU</h2>

      <div class="row d-flex justify-content-center align-items-center">
        <!-- Cover Buku -->
        <div class="col-md-4 text-center">
          <img :src="buku?.cover" class="cover img-fluid rounded shadow" alt="Cover Buku" style="max-width: 250px;">
        </div>

        <!-- Detail Buku -->
        <div class="col-md-7 text-white">
          <h1 class="text-center my-3">{{ buku?.judul }}</h1>
          <div class="book-details">
            <p><strong>Penulis:</strong> {{ buku?.penulis }}</p>
            <p><strong>Penerbit:</strong> {{ buku?.penerbit }}</p>
            <p><strong>Tahun Terbit:</strong> {{ buku?.tahun_terbit }}</p>
            <p><strong>Rak:</strong> {{ buku?.rak }}</p>
            <p><strong>Kategori:</strong> {{ buku?.kategori_buku?.nama }}</p>
            <p><strong>Deskripsi:</strong> {{ buku?.deskripsi }}</p>
          </div>
        </div>
      </div>

      <!-- Tombol Kembali -->
      <div class="row d-flex justify-content-center mt-4">
        <nuxt-link to="/buku" class="btn-custom">Kembali</nuxt-link>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';

const supabase = useSupabaseClient();
const route = useRoute();
const buku = ref();
const kategories = ref([]);

const getBookById = async () => {
  const { data, error } = await supabase
    .from('buku')
    .select(`*, kategori_buku(*)`)
    .eq('id', route.params.id)
    .maybeSingle();
  if (data) buku.value = data;
};

const getKategori = async () => {
  const { data } = await supabase.from('kategori_buku').select('*');
  if (data) kategories.value = data;
};

onMounted(() => {
  getBookById();
  getKategori();
});
</script>

<style scoped>
.content {
  background-color: #6AB187;
  background-size: cover;
  width: 100%;
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: black;
  padding-bottom: 50px;
}

.book-details p {
  font-size: 1.2rem;
  margin: 5px 0;
}

.btn-custom {
  display: inline-block;
  background-color: white;
  color: black;
  width: 160px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  border-radius: 10px;
  font-weight: bold;
  text-decoration: none;
}
</style>
