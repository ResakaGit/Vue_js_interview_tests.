<script setup lang="ts">
import TableComponet from './components/TableComponet.vue'
import { ref, onMounted, } from 'vue'
import axios, { AxiosRequestConfig, } from 'axios'

interface CharactersResponse {
  info: {
    count: number,
    pages: number,
    next: URL,
    prev: null
  },

  results: [
    {
      id: number,
      name: string,
      status: string,
      species: string,
      type: string,
      gender: string,
      origin: {
        name: string,
        url: URL
      },
      location: {
        name: string,
        url: URL
      },
      image: URL,
      episode: [URL],
      url: URL,
      created: Date
    },
  ]
}

const url = "https://rickandmortyapi.com/api/character/"
const dataByGet = ref<CharactersResponse>()
const loadign = ref<boolean>(true);



const changePage = (page: number) => {
  console.log("page", page)
  getRickAndMortyApi(url, page)
}


async function getRickAndMortyApi(URL: string, page: number, limit: number = 10, config?: AxiosRequestConfig) {
  loadign.value = true;
  dataTable.value = [];
  const arrayForGet = []
  if (limit) {
    for (let i = 1; i < limit + 1; i++) {
      arrayForGet.push(i + (page * limit))
    };
  }
  try {
    let res = await axios.get(`${URL}/${limit ? arrayForGet : 1}`, config)
    console.log("Response by getRickAndMortyApi ", res)
    loadign.value = res.data && true;
    dataTable.value = res.data
  } catch (error) {
    alert(`Error in Fetch, Message: ${error}`)
  }
}

const dataTable = ref<any>([]);

onMounted(async () => {
  await getRickAndMortyApi(url, 1);
})



</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
    <a href="https://www.typescriptlang.org/docs/" target="_blank">
      <img src="./assets/typescript.svg" class="logo typescript" alt="Vue logo" />
    </a>
  </div>
  <TableComponet :v-if="loadign" tableName="Vue + vite + typescript" :tableData="dataTable" @changePage="changePage" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.logo.tipescript:hover {
  filter: drop-shadow(0 0 2em #007acc);
}
</style>
