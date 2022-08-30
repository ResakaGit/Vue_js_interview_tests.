<script setup lang="ts">
import  TableRickAndMorty from './components/TableByRickAndMorty.vue'
import TableByjsonplaceholderVue from './components/TableByjsonplaceholder.vue';
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
interface jsonplaceholderResponse {
  userId: number,
  id: number,
  title: string,
  body: string
}

const rickMortyApiurl = "https://rickandmortyapi.com/api/character/"
const jsonpPlaceholderApiurl = "https://jsonplaceholder.typicode.com/posts"
const loadign = ref<boolean>(true);
const rickAndMortydataTable = ref<CharactersResponse | any>([]);
const jsonpPlaceholderdataTable = ref<jsonplaceholderResponse | any>([]);
const tableSelect = ref<string>("jsonpPlaceholder");


const changePageJsonpPlaceholder = (page: number) => {
  getJsonPlaceholderApi(jsonpPlaceholderApiurl, page)
}
const changePagerRickAndMorty = (page: number) => {
  getRickAndMortyApi(rickMortyApiurl, page)
}


const SeletTable = (name: string) => {
  tableSelect.value = name;
}

const getRickAndMortyApi = async (URL: string, page: number, limit: number = 10, config?: AxiosRequestConfig) => {
  loadign.value = true;
  rickAndMortydataTable.value = [];
  const arrayForGet = []
  if (limit) {
    for (let i = 1; i < limit + 1; i++) {
      arrayForGet.push(i + (page * limit))
    };
  }
  try {
    let res = await axios.get(`${URL}/${limit ? arrayForGet : 1}`, config)
    loadign.value = res.data && true;
    rickAndMortydataTable.value = res.data
  } catch (error) {
    alert(`Error in Fetch, Message: ${error}`)
  }
}

const getJsonPlaceholderApi = async (URL: string, page: number, config?: AxiosRequestConfig) => {
  loadign.value = true;
  jsonpPlaceholderdataTable.value = [];
  console.log(`${URL}/${page}/comments`)
  try {
    let res = await axios.get(`${URL}/${page}/comments`, config)
    console.log("Response by jsonpPlaceholder ", res)
    loadign.value = res.data && true;
    jsonpPlaceholderdataTable.value = res.data
  } catch (error) {
    alert(`Error in Fetch, Message: ${error}`)
  }
}


onMounted(async () => {
  await getRickAndMortyApi(rickMortyApiurl, 1);
  await getJsonPlaceholderApi(jsonpPlaceholderApiurl, 1);
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
  <!-- buton to change the components -->
  <div class="buttons">
    <button class="Button001" @click="SeletTable('rickAndMorty')">Rick And Morty</button>
    <button class="Button001" @click="SeletTable('jsonpPlaceholder')">JsonPlaceholder</button>
  </div>
  <div v-if="tableSelect === 'rickAndMorty'" style="width:100% ;">
    <TableRickAndMorty :v-if="loadign" tableName="Vue + vite + typescript" :tableData="rickAndMortydataTable"
      @changePageRickAndMorty="changePagerRickAndMorty" />
  </div>
  <div v-else-if="tableSelect === 'jsonpPlaceholder'" style="width:100% ;">
    <TableByjsonplaceholderVue :v-if="loadign" :tableData="jsonpPlaceholderdataTable" tableName="Vue + Element Ui"
      @changePageJsonpPlaceholder="changePageJsonpPlaceholder" />
  </div>
</template>

<style scoped>
.Button001 {
  border: 1px solid #41b883;
  color: #41b883;
  text-shadow: 5px 3px 3px #35495e;
  font-size: 1em;
}

/* margin in buttons  */
.buttons {
  margin-top: 1em;

}

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
