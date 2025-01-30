<template>
  <div class="container-fluid table-responsive">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4 pt-3">RIWAYAT KUNJUNGAN</h2>
        <div class="my-3">Menampilkan {{ visitors.length }} dari {{ visitors.length }}</div>
        <div class="table-responsive">
          <table class="table table-bordered border-white text-white">
            <thead class="table-dark">
              <tr>
                <th>No</th>
                <th>Tanggal</th>
                <th>Waktu</th>
                <th>Nama</th>
                <th>Keanggotaan</th>
                <th>Kelas</th>
                <th>Keperluan</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(visitor, i) in visitors" :key="visitor.id">
                <td>{{ i + 1 }}.</td>
                <td>{{ visitor.tanggal }}</td>
                <td>{{ visitor.waktu }}</td>
                <td>{{ visitor.nama }}</td>
                <td>{{ visitor.keanggotaan.nama }}</td>
                <td>{{ visitor.tingkat }}-{{ visitor.jurusan }}{{ visitor.kelas }}</td>
                <td>{{ visitor.keperluan.nama }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-12 d-flex justify-content-end">
        <nuxt-link to="/pengunjung/menu" class="btn-custom">Menu</nuxt-link>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const visitors = ref([]);

const getPengunjung = async () => {
  const { data } = await supabase
    .from('pengunjung')
    .select(`*, keanggotaan(*), keperluan(*)`)
    .order('tanggal', { ascending: false }); // Data terbaru di atas

  if (data) visitors.value = data;
};

onMounted(() => {
  getPengunjung();
});
</script>

<style scoped>
.container-fluid {
  background-image: url('@/assets/img/home.png');
  background-size: cover;
  height: 100vh;
  width: 100%;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: white;
}

.table th,
.table td {
  text-align: center;
  padding: 10px;
}

.btn-custom {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: white;
  color: black;
  width: 150px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  text-decoration: none;
  font-size: 1.2rem;
  font-weight: bold;
}
</style>
