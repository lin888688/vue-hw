<template>
  <src></src>
  <table class="table table-hover table-bordered">
    <thead>
      <tr>
        <th scope="col">站點編號</th>
        <th scope="col">站點名稱</th>
        <th scope="col">站點所在區域</th>
        <th scope="col">站點地址</th>
        <th scope="col">總車位數量</th>
        <th scope="col">可租借的腳踏車數量</th>
        <th scope="col">站點緯度</th>
        <th scope="col">站點經度</th>
        <th scope="col">可歸還的腳踏車數量</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="bike in bikes" :key="bike.sno">
        <td scope="col">{{ bike.sno }}</td>
        <td scope="col">{{ bike.sna }}</td>
        <td scope="col">{{ bike.sarea }}</td>
        <td scope="col">{{ bike.ar }}</td>
        <td scope="col">{{ bike.total }}</td>
        <td scope="col">{{ bike.available_rent_bikes }}</td>
        <td scope="col">{{ bike.latitude }}</td>
        <td scope="col">{{ bike.longitude }}</td>
        <td scope="col">{{ bike.available_return_bikes }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import json from '@/assets/youbike_immediate.json';
const bikes = ref([]);
onMounted(() => {
  bikes.value = json;
});
</script>

<style scoped></style>
