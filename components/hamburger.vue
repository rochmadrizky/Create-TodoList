<template>
  <header class="p-3 bg-gray-100 opacity-70 sticky top-0 left-0 right-0 z-10">
    <div
      class="container md:px-20 mx-auto flex justify-between items-center lg:flex-col"
    >
      <div class="text-2xl font-bold mb-2">ToDo List</div>

      <!-- Hamburger button (sekarang berada di pojok kanan atas) -->
      <button @click="toggleMenu" class="lg:hidden relative ml-60">
        <component :is="isSidebarOpen ? IconX : IconMenu2"></component>
      </button>

      <div
        v-if="isSidebarOpen"
        @click="toggleMenu"
        class="fixed inset-0 bg-black opacity-40 z-40"
      ></div>

      <div :class="sidebarClasses">
        <div class="py-6">
          <a href="#">SimoLabs</a>
        </div>
        <MenuNav />
      </div>

      <div class="hidden lg:block">
        <MenuNav />
      </div>
    </div>
  </header>
</template>

<script setup>
import { IconMenu2 } from "@tabler/icons-vue";
import { IconX } from "@tabler/icons-vue";
import MenuNav from "./MenuNav.vue";
import { ref, computed } from "vue";

// Menggunakan ref untuk mengelola status sidebar yang terbuka atau tertutup
const isSidebarOpen = ref(false);

// Fungsi untuk mengganti status sidebar
const toggleMenu = () => {
  isSidebarOpen.value = !isSidebarOpen.value;
};

// Computed property untuk mengganti kelas CSS pada elemen sidebar
const sidebarClasses = computed(() => {
  return (
    "fixed top-0 left-0 h-full w-64 md:w-72 lg:w-[600px] bg-gray-100 text-black px-4 transform transition-transform duration-300 ease-in-out z-50" +
    (isSidebarOpen.value ? " translate-x-0" : " -translate-x-full")
  );
});
</script>
