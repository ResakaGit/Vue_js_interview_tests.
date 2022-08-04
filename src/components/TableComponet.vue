<script setup lang="ts" >
import { ref } from 'vue'

export type DataPropsKeys =
    'id' |
    'name' |
    'status' |
    'species' |
    'type' |
    'gender' |
    'origin' |
    'location' |
    'image' |
    'episode' |
    'url' |
    'created' |
    'name' |
    'url'






export interface DataProps {
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
}

const deafaltDataTable = [{
    id: 1,
    name: "sogia",
    status: "Alive",
    species: "Human",
    type: "principal",
    gender: "Male",
    origin: {
        name: "Earth",
        url: "https://rickandmortyapi.com/api/location/1"
    },
    location: {
        name: "Earth",
        url: "https://rickandmortyapi.com/api/location/20"
    },
    image: "https://rickandmortyapi.com/api/character/avatar/1.jpeg",
    episode: [
        "https://rickandmortyapi.com/api/episode/1",
    ],
    url: "https://rickandmortyapi.com/api/character/1",
    created: "2017-11-04T18:48:46.250Z"
}, {
    id: 1,
    name: "aguusto",
    status: "Alive",
    species: "Human",
    type: "secundario",
    gender: "Male",
    origin: {
        name: "Earth",
        url: "https://rickandmortyapi.com/api/location/1"
    },
    location: {
        name: "Earth",
        url: "https://rickandmortyapi.com/api/location/20"
    },
    image: "https://rickandmortyapi.com/api/character/avatar/1.jpeg",
    episode: [
        "https://rickandmortyapi.com/api/episode/1",
    ],
    url: "https://rickandmortyapi.com/api/character/1",
    created: "2017-11-04T18:48:46.250Z"
}, {
    id: 1,
    name: "Rick Sanchez",
    status: "Alive",
    species: "Human",
    type: "secundario",
    gender: "Male",
    origin: {
        name: "Earth",
        url: "https://rickandmortyapi.com/api/location/1"
    },
    location: {
        name: "Earth",
        url: "https://rickandmortyapi.com/api/location/20"
    },
    image: "https://rickandmortyapi.com/api/character/avatar/1.jpeg",
    episode: [
        "https://rickandmortyapi.com/api/episode/1",
        "https://rickandmortyapi.com/api/episode/2",
        "https://rickandmortyapi.com/api/episode/3",
    ],
    url: "https://rickandmortyapi.com/api/character/1",
    created: "2017-11-04T18:48:46.250Z"
}]
const props = defineProps<{
    tableName?: string, tableData?: DataProps[]
}>()
const emits = defineEmits(["changePage"])
const page = ref<number>(0);


const nextAndBackPage = (accion: string) => {
    if (page.value === 0 && accion !== 'next') { return alert(`you are already on the first page`) };
    accion === 'next' ? page.value++ : page.value--;

    emits("changePage", page.value)
}

const handelChangeTable = (e: any, index: number) => {
    deafaltDataTable[index] = { ...deafaltDataTable[index], [e.target.name]: e.target.value };
}

const handelChangeTableLocaliti = (e: any, index: number) => {
    deafaltDataTable[index].location = { ...deafaltDataTable[index].location, [e.target.name]: e.target.value };
}

const handelChangeTableepisode = (e: any, index: number) => {
    deafaltDataTable[index].episode[index] = e.target.value
}


</script>

<template>
    <div class="container_div">
        <h1 class="TableName001">{{ tableName ?? "Table name here" }}</h1>
        <table class="table001">
            <tr>
                <button type="button" @click="nextAndBackPage('previus')">Previous page</button>
                <button type="button" @click="nextAndBackPage('next')">Next page</button>

            </tr>
            <tr>
                <th>Nombre</th>
                <th>Status</th>
                <th>ID</th>
                <th>Genero</th>
                <th>Planeta</th>
                <th>episode</th>
                <th>URL</th>
                <th>created</th>

            </tr>
            <tr v-for="(item, index) in tableData ?? deafaltDataTable">
                <td> <input v-on:change="handelChangeTable($event, index)" type="text" name="name" v-model="item.name">
                </td>
                <td> <input v-on:change="handelChangeTable($event, index)" type="text" name="status"
                        v-model="item.status">
                </td>
                <td> <input v-on:change="handelChangeTable($event, index)" type="text" name="id" v-model="item.id"></td>
                <td> <input v-on:change="handelChangeTable($event, index)" type="text" name="gender"
                        v-model="item.gender">
                </td>
                <td> <input v-on:change="handelChangeTableLocaliti($event, index)" type="text" name="name"
                        v-model="item.location.name">
                </td>
                <td>
                    <input v-for="(episodes, i) in item.episode" v-on:change="handelChangeTableepisode($event, index)"
                        type="text" name="created" v-model="item.episode[i]">
                </td>
                <td> <input v-on:change="handelChangeTable($event, index)" type="text" name="url" v-model="item.url">
                </td>
                <td> <input v-on:change="handelChangeTable($event, index)" type="text" name="created"
                        v-model="item.created">
                </td>
            </tr>
        </table>
        <div>
        </div>
    </div>
</template>




<style scoped>
.container_div {
    position: relative;
    align-items: center;
}

.table001 {
    margin-top: 10%;
}

.TableName001 {
    border: 1px solid #41b883;
    color: #41b883;
    text-shadow: 5px 3px 3px #35495e;
    font-size: 7.5em;
}

/* #ffce26, 9px 9px 9px#2f74c0,13px 13px 13px #41b883 */
table {
    display: block;
    overflow-x: auto;
    white-space: nowrap;
    scroll-margin-top: 100px;
}

th,
td {
    margin: auto;
    border: 1px solid #41b883;
    min-width: 100px;
    align-items: center;

}

input {
    border: 1px solid #41b883;
}
</style>