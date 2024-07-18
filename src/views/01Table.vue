<template>
  <div class="container text-center">
    <div class="row align-items-center">
      <table class="table table-hover table-bordered table-striped" style="font-size: 95%">
        <thead>
          <tr>
            <th scope="col">站點編號</th>
            <th scope="col">站點名稱</th>
            <th scope="col">站點所在區域</th>
            <th scope="col">站點地址</th>

            <th scope="col">
              總車位數量
              <img src="/arrow-down-short.svg" style="width: 20px" @click="sortByDown('total')" />
              <img src="/arrow-up-short.svg" style="width: 20px" @click="sortByUp('total')" />
            </th>

            <th scope="col">
              可租借的腳踏車數量
              <img
                src="/arrow-down-short.svg"
                style="width: 20px"
                @click="sortByDown('available_rent_bikes')"
              />
              <img
                src="/arrow-up-short.svg"
                style="width: 20px"
                @click="sortByUp('available_rent_bikes')"
              />
            </th>

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
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import json from '@/assets/youbike_immediate.json';

const bikes = ref([]);

onMounted(() => {
  bikes.value = json;
});

function sortByDown(key) {
  bikes.value.sort((a, b) => a[key] - b[key]);
}

function sortByUp(key) {
  bikes.value.sort((a, b) => b[key] - a[key]);
}
</script>

<style scoped></style>
