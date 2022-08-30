<!-- setup script the standar for vue 3 -->

<script setup lang="ts">
import { ref } from 'vue'
/* iNTERFACE FOR RESPONSE */
interface jsonplaceholderResponse {
    name: number,
    id: number,
    email: string,
    body: string
}
const props = defineProps<{
    tableName?: string, tableData?: jsonplaceholderResponse[]
}>()


/* Definitios */
const emits = defineEmits(["changePageJsonpPlaceholder"])
const page = ref<number>(1);
const dialogTableVisible = ref(false);
const userSelected = ref();

/* To get news users */
const nextAndBackPage = (accion?: string) => {
    if (page.value === 1 && accion !== 'next') { return alert(`you are already on the first page`) };
    accion === 'next' ? page.value++ : page.value--;
    emits("changePageJsonpPlaceholder", page.value);
}


const deleteRow = (index: number) => {
    if (props.tableData) {
        props.tableData.splice(index, 1)
    }
}

const handleSelectRow = (user: jsonplaceholderResponse) => {
    userSelected.value = user;
    dialogTableVisible.value = true;
    console.log(userSelected);
}

</script>
<template>
    <!-- table and titule -->
    <h1 class="TableName001" >{{  tableName ? tableName : 'Name here'  }}</h1>
    <div class="divElementUi001">
        <el-button type="success" @click="nextAndBackPage()">Back page </el-button>
        <el-button type="success" @click="nextAndBackPage('next')">Next page</el-button>
    </div>

    <el-table :data="tableData">
        <el-table-column align="center" width=100%>
            <el-table-column prop="id" label="id" width="150" />
            <el-table-column prop="name" label="Name" />
            <el-table-column prop="email" label="email" />
            <el-table-column fixed="right" label="Operations" width="120">
                <template #default="scope">
                    <el-button link type="primary" size="small" @click="handleSelectRow(scope.row)">
                        Info
                    </el-button>
                    <el-button link type="danger" size="small" @click.prevent="deleteRow(scope.$index)">
                        Remove
                    </el-button>
                </template>
            </el-table-column>
        </el-table-column>
    </el-table>
    <!-- Model for show data -->
    <el-dialog v-model="dialogTableVisible" title="Current user">
        <el-form v-if="dialogTableVisible" label-width="80px" :model="userSelected">
            <el-form-item label="id">
                <el-input v-model="userSelected.id" placeholder="id" />
            </el-form-item>
            <el-form-item label="name">
                <el-input v-model="userSelected.name" placeholder="name" />
            </el-form-item>
            <el-form-item label="email">
                <el-input v-model="userSelected.email" placeholder="email" />
            </el-form-item>
            <el-form-item label="body">
                <el-input v-model="userSelected.body" type="textarea" :rows="3" placeholder="body" />
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="dialogTableVisible = false">Cancel</el-button>
            <el-button type="primary" @click="dialogTableVisible = false">Confirm</el-button>
        </div>
    </el-dialog>
</template>
<style scoped>
.divElementUi001 {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 1000px;
}

.TableName001 {
    border: 1px solid #41b883;
    color: #41b883;
    text-shadow: 5px 3px 3px #35495e;
    font-size: 7.5em;
}
</style>