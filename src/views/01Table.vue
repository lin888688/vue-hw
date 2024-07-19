<template>
  <div style="width: 90%; margin: auto">
    <nav class="navbar bg-body-tertiary" style="margin-top: 8%">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">UBike</a>
        <form class="d-flex" role="search" @submit.prevent="searchBikes">
          <input
            class="form-control me-2"
            type="search"
            placeholder="輸入站點地址"
            aria-label="Search"
            v-model="request"
            @input="searchBikes"
          />
          <button class="btn btn-outline-primary" type="submit">Search</button>
        </form>
      </div>
    </nav>

    <div class="text-center">
      <div class="align-items-center">
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
            <tr v-for="bike in resultBikes" :key="bike.sno">
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
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const bikes = ref([]);

const request = ref('');
const resultBikes = ref([]);

function searchBikes() {
  const result = request.value.trim();
  if (!result) {
    resultBikes.value = bikes.value;
    return;
  }
  const regex = new RegExp(result, 'gi');
  resultBikes.value = bikes.value.filter((bike) => [...bike.ar.matchAll(regex)].length > 0);
}

onMounted(() => {
  axios
    .get('https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json')
    .then((response) => {
      bikes.value = response.data;
      resultBikes.value = response.data;
    });
});

function sortByDown(key) {
  resultBikes.value.sort((a, b) => a[key] - b[key]);
}

function sortByUp(key) {
  resultBikes.value.sort((a, b) => b[key] - a[key]);
}
</script>

<style scoped>
.result-Exist {
  color: red;
}
</style>
