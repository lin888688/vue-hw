<template>
  <nav aria-label="...">
    <ul class="pagination" :page-count="pages">
      <li class="page-item" :disabled="isDisabled">
        <a class="page-link">上一頁</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">1</a>
      </li>
      <li class="page-item" aria-current="page">
        <a class="page-link" href="#">2</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">3</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">Next</a>
      </li>
    </ul>
  </nav>
</template>

<script setup>
import { ref, computed } from 'vue';
const page = ref(0);
const pages = ref(10);
const isDisabled = computed(() => page.value == 1);
</script>

<style scoped></style>
