<template>
  <div class="content">
    <div class="form-container">
      <h2 class="text-center mb-4">ISI DATA KUNJUNGAN</h2>

      <form @submit.prevent="kirimData">
        <div class="mb-3">
          <input v-model="form.nama" type="text" class="form-control form-control-lg rounded-5" placeholder="Nama..." required>
        </div>

        <div class="mb-3">
          <select v-model="form.keanggotaan" @change="resetKelas" class="form-control form-control-lg form-select rounded-5" required>
            <option value="">Keanggotaan...</option>
            <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
          </select>
        </div>

        <div v-if="form.keanggotaan == '1'" class="mb-3">
          <div class="row">
            <div class="col-md-4">
              <select v-model="form.tingkat" class="form-control form-control-lg form-select rounded-5 mb-2" required>
                <option value="">Tingkat...</option>
                <option value="X">X</option>
                <option value="XI">XI</option>
                <option value="XII">XII</option>
              </select>
            </div>

            <div class="col-md-4">
              <select v-model="form.jurusan" class="form-control form-control-lg form-select rounded-5 mb-2" required>
                <option value="">Jurusan...</option>
                <option value="PPLG">PPLG</option>
                <option value="TJKT">TJKT</option>
                <option value="TSM">TSM</option>
                <option value="DKV">DKV</option>
                <option value="TOI">TOI</option>
              </select>
            </div>

            <div class="col-md-4">
              <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-5 mb-2" required>
                <option value="">Kelas...</option>
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
              </select>
            </div>
          </div>
        </div>

        <div class="mb-3">
          <select v-model="form.keperluan" class="form-control form-control-lg form-select rounded-5" required>
            <option value="">Keperluan...</option>
            <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
          </select>
        </div>

        <button type="submit" class="btn btn-submit rounded-5 px-5">Kirim</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useSupabaseClient } from '#imports';

const supabase = useSupabaseClient();

const members = ref([]);
const objectives = ref([]);
const form = ref({
  nama: "",
  keanggotaan: "",
  tingkat: "",
  jurusan: "",
  kelas: "",
  keperluan: ""
});

const kirimData = async () => {
  const { error } = await supabase.from('pengunjung').insert([form.value]);
  if (error) throw error;
  else navigateTo('/pengunjung/riwayat');
};

const getKeanggotaan = async () => {
  const { data, error } = await supabase.from('keanggotaan').select('*');
  if (data) members.value = data;
};

const getKeperluan = async () => {
  const { data, error } = await supabase.from('keperluan').select('*');
  if (data) objectives.value = data;
};

const resetKelas = () => {
  if (["2", "3", "4"].includes(form.value.keanggotaan)) {
    form.value.tingkat = "";
    form.value.jurusan = "";
    form.value.kelas = "";
  }
};

onMounted(() => {
  getKeanggotaan();
  getKeperluan();
});
</script>

<style scoped>
/* Menempatkan form ke tengah */
.content {
  background-color: #6AB187;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Form container */
.form-container {
  background-color: #20948B;
  padding: 3rem;
  border-radius: 20px;
  color: white;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  text-align: center;
}

/* Tombol submit */
.btn-submit {
  background-color: #ffffff;
  color: black;
  width: 150px;
  height: 50px;
  font-weight: bold;
  transition: background 0.3s, transform 0.2s;
}

/* Efek hover */
.btn-submit:hover {
  background-color: #f0f0f0;
  transform: scale(1.05);
}

/* Placeholder styling */
::placeholder {
  color: rgba(0, 0, 0, 0.6);
  font-size: 1rem;
}
</style>
