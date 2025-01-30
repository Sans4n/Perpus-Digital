<template>
  <div class="content d-flex justify-content-center align-items-center">
    <div class="container">
      <div class="row justify-content-center py-5">
        <!-- Kartu Riwayat Kunjungan -->
        <div class="col-md-6 col-sm-12 mb-3">
          <nuxt-link to="/pengunjung/riwayat">
            <div class="card bg-pengunjung rounded-5 shadow">
              <div class="card-body d-flex flex-column justify-content-center align-items-center">
                <h2 class="text-center">RIWAYAT KUNJUNGAN</h2>
              </div>
            </div>
          </nuxt-link>
        </div>

        <!-- Kartu Cari Buku -->
        <div class="col-md-6 col-sm-12">
          <nuxt-link to="/buku">
            <div class="card bg-buku rounded-5 shadow">
              <div class="card-body d-flex flex-column justify-content-center align-items-center">
                <h2 class="text-center">CARI BUKU</h2>
              </div>
            </div>
          </nuxt-link>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Background Utama */
.content {
  background-color: #6AB187;
  width: 100%;
  min-height: 100vh;
}

/* Kartu */
.card {
  height: 300px;
  opacity: 0.9;
  color: white;
  text-decoration: none;
  transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
}

/* Efek Hover */
.card:hover {
  transform: scale(1.05);
  opacity: 1;
}

/* Gaya Latar Belakang Kartu */
.card.bg-pengunjung {
  background-image: url('@/assets/img/bg-home-kunjungan.jpg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.card.bg-buku {
  background-image: url('@/assets/img/bg-home-cari-buku.jpg');
  background-size: cover;
}
</style>
