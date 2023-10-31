<template>
  <div class="mt-4 max-w-md mx-auto bg-white p-6 rounded shadow-lg">
    <h1 class="text-2xl font-semibold mb-4">Buat catatan:</h1>
    <div class="my-4">
      <input
        v-model="tugasBaru"
        class="w-full p-2 border rounded"
        placeholder="Tambahkan catatan disini..."
        @keyup.enter="tambahkanTugasBaru"
      />
      <!-- tombol menambahkan tugas baru -->
      <div class="pt-6 flex items-center justify-center">
        <button
          @click="tambahkanTugasBaru"
          class="bg-blue-500 text-white py-2 px-4 rounded"
          :class="{
            'bg-gray-400 cursor-not-allowed': tugasBaru.trim() === '',
            'bg-blue-500': tugasBaru.trim() !== '',
          }"
          :disabled="tugasBaru.trim() === ''"
        >
          Tambahkan
        </button>
      </div>
    </div>
  </div>

  <div class="my-6 max-w-md mx-auto bg-white p-6 rounded shadow-lg">
    <h1 class="text-2xl font-semibold mb-4">
      Daftar catatan yang telah dibuat:
    </h1>
    <ul>
      <li
        v-for="(task, index) in tasks"
        :key="index"
        :class="{
          'bg-green-500': task.completed,
          'bg-white': !task.completed,
        }"
        class="flex justify-between items-center w-full p-2 border rounded"
      >
        <div class="flex items-center">
          <input
            type="checkbox"
            @change="handleCheckbox"
            v-model="tasks[index].completed"
            class="mr-2"
          />
          <span>{{ task.isi }}</span>
        </div>
        <!-- tombol hapus tugas -->
        <button @click="konfirmasiHapusTugas(index)" class="text-red-500">
          Hapus
        </button>
      </li>

      <p
        v-if="tasks.length === 0"
        class="w-full p-2 border rounded text-gray-400 text-center"
      >
        Tidak ada list...
      </p>
    </ul>

    <!-- ini bagian terakhir -->
    <div class="flex items-center justify-center gap-6">
      <!-- ini tombol selesai -->
      <button
        @click="tombolSemuaSelelsai"
        class="mt-4 bg-green-500 text-white py-2 px-4 rounded"
      >
        Selesai semua
      </button>
      <!-- ini tombol hapus semua -->
      <button
        @click="konfirmasiHapusSemuaTugas"
        @keyup.enter="konfirmasiHapusSemuaTugas"
        class="mt-4 bg-red-500 text-white py-2 px-4 rounded"
      >
        Hapus Semua
      </button>
    </div>
  </div>
</template>

<script setup>
const tasks = ref([]);
const tugasBaru = ref("");

// Menggunakan local storage key yang unik untuk tugas.
const localStorageTugas = "toDoTugas";

const handleCheckbox = () => {
  simpanTugasDiLocalStorage();
};

// Fungsi untuk menyimpan tugas ke local storage.
const simpanTugasDiLocalStorage = () => {
  localStorage.setItem(localStorageTugas, JSON.stringify(tasks.value));
};

// Fungsi untuk memuat tugas dari local storage.
const loadTugasDariLocalStorage = () => {
  const simpanTugas = localStorage.getItem(localStorageTugas);
  if (simpanTugas) {
    tasks.value = JSON.parse(simpanTugas);
  }
};

// Memanggil fungsi memuat saat komponen dimuat.
onMounted(() => {
  loadTugasDariLocalStorage();
});

const semuaTugasSelesai = computed(() =>
  tasks.value.every((task) => task.completed)
);

const tambahkanTugasBaru = () => {
  if (tugasBaru.value.trim() !== "") {
    tasks.value.push({ isi: tugasBaru.value, completed: false });
    tugasBaru.value = "";
    simpanTugasDiLocalStorage(); // Simpan tugas ke local storage setelah menambahkan.
  }
};

const hapusTugas = (index) => {
  tasks.value.splice(index, 1);
  simpanTugasDiLocalStorage(); // Simpan tugas ke local storage setelah menghapus.
};

const tombolSemuaSelelsai = () => {
  if (!semuaTugasSelesai.value) {
    tasks.value.forEach((task) => {
      task.completed = true;
    });
  }
  simpanTugasDiLocalStorage(); // Simpan tugas ke local storage setelah mengubah status.
};

const konfirmasiHapusSemuaTugas = () => {
  if (confirm("Yakin dihapus semua?")) {
    hapusSemuaTugas();
  }
};

const konfirmasiHapusTugas = (index) => {
  const taskText = tasks.value[index].isi;
  if (confirm(`Yakin ${taskText} dihapus?`)) {
    hapusTugas(index);
  }
};

const hapusSemuaTugas = () => {
  tasks.value = [];
  simpanTugasDiLocalStorage(); // Simpan tugas ke local storage setelah menghapus semua.
};
</script>
