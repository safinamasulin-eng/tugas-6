<script setup>
import { ref, computed } from 'vue'

const judul = ref('')
const penulis = ref('')
const kategori = ref('Novel')
const sudahDibacaBaru = ref(false)

const daftarBuku = ref([
  { id: 1, judul: 'Laskar Pelangi', penulis: 'Andrea Hirata', kategori: 'Novel', sudahDibaca: true },
  { id: 2, judul: 'Bumi Manusia', penulis: 'Pramoedya Ananta Toer', kategori: 'Sejarah', sudahDibaca: false },
])

// INI YANG DIMINTA TUGAS: computed
const jumlahSudahDibaca = computed(() => {
  return daftarBuku.value.filter(buku => buku.sudahDibaca).length
})

const totalBuku = computed(() => daftarBuku.value.length)

function tambahBuku() {
  if (!judul.value || !penulis.value) {
    alert('Judul dan Penulis harus diisi!')
    return
  }
  daftarBuku.value.push({
    id: Date.now(),
    judul: judul.value,
    penulis: penulis.value,
    kategori: kategori.value,
    sudahDibaca: sudahDibacaBaru.value
  })
  judul.value = ''
  penulis.value = ''
  kategori.value = 'Novel'
  sudahDibacaBaru.value = false
}

function hapusBuku(id) {
  daftarBuku.value = daftarBuku.value.filter(buku => buku.id !== id)
}
</script>

<template>
  <div>
    <h2>Catatan Buku Saya</h2>
    <p>Total: {{ totalBuku }} | Sudah Dibaca: {{ jumlahSudahDibaca }}</p>

    <input v-model="judul" placeholder="Judul Buku" />
    <input v-model="penulis" placeholder="Nama Penulis" />

    <!-- KATEGORI - SELECT DROPDOWN -->
    <select v-model="kategori">
      <option>Novel</option>
      <option>Komik</option>
      <option>Sejarah</option>
      <option>Teknologi</option>
      <option>Lainnya</option>
    </select>

    <!-- STATUS BACA - CHECKBOX -->
    <label>
      <input type="checkbox" v-model="sudahDibacaBaru" />
      Sudah Dibaca
    </label>

    <button @click="tambahBuku">+ Tambah Buku</button>

    <ul>
      <li v-for="buku in daftarBuku" :key="buku.id">
        <b>{{ buku.judul }}</b> - {{ buku.penulis }} - {{ buku.kategori }} -
        <input type="checkbox" v-model="buku.sudahDibaca" />
        {{ buku.sudahDibaca ? 'Sudah Dibaca' : 'Belum Dibaca' }}
        <button @click="hapusBuku(buku.id)">Hapus</button>
      </li>
    </ul>
  </div>
</template>