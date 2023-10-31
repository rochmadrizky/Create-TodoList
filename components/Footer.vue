<template>
  <footer
    class="max-w-4xl py-4 text-center md:text-right md:pr-6 text-lg mdtext-xl font-seconds bottom-0"
  >
    Copyright &copy;{{ Tahun }} All rights reserved.
  </footer>
</template>

<script setup>
const Tahun = new Date().getFullYear();
</script>
