<template>
  <div style="width: 90%; margin: auto">
    <nav class="navbar bg-body-tertiary" style="margin-top: 8%">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">UBike</a>
        <form class="d-flex" role="search" @submit.prevent="searchBikes">
          <input class="form-control me-2" type="search" placeholder="輸入站點地址" aria-label="Search" v-model="request" />
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
                <img src="/arrow-down-short.svg" style="width: 20px" @click="sortByDown('available_rent_bikes')" />
                <img src="/arrow-up-short.svg" style="width: 20px" @click="sortByUp('available_rent_bikes')" />
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
              <td scope="col">
                <span v-for="part in highlight(bike.ar)" :class="{ 'result-Exist': part.highlight }" :key="part">
                  {{ part.word }}
                </span>
              </td>
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
import { ref, onMounted, computed } from 'vue';

const bikes = ref([]);

const request = ref('');


const resultBikes = computed(() => {
  const result = request.value.trim();
  if (!result) return bikes.value;

  const regex = new RegExp(result, 'gi');
  return bikes.value.filter((bike) => [...bike.ar.matchAll(regex)].length > 0);
});

function highlight(word) {
  const result = request.value.trim();
  const index = word.indexOf(result);

  if (index == -1) return [{ word, highlight: false }];

  return [
    { word: word.slice(0, index), highlight: false },
    { word: word.slice(index, index + result.length), highlight: true },
    { word: word.slice(index + result.length), highlight: false }
  ];

}

onMounted(async () => {

  const response = await fetch('https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json');
  const data = await response.json();
  bikes.value = data;

});

function sortByDown(key) {
  bikes.value.sort((a, b) => a[key] - b[key]);
}

function sortByUp(key) {
  bikes.value.sort((a, b) => b[key] - a[key]);
}

</script>

<style scoped>
.result-Exist {
  color: red;
}
</style>
