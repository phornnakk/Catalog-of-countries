<template>
    <div class="">
        <pre>
          ssss  {{ keywords }}
        </pre>
        <el-row style="align-items: center;">
            <el-col :span="12">
                <h1>Countries Catalog</h1>
            </el-col>
            <el-col :span="12">
                <el-input v-model="keywords" placeholder="Country Name" :prefix-icon="Search" />
            </el-col>
        </el-row>

        <p>Sort By Name</p>
        <el-row style="align-items: center;">
            <el-col :span="12">
                <el-button type="primary" plain>Ascending</el-button>
                <el-button type="primary" plain>Descending</el-button>
                <el-button type="primary">Reset</el-button>
            </el-col>
            <el-col :span="12" style="text-align: right;">
                <el-button type="primary">Previous</el-button>
                <el-button type="primary">Next</el-button>

            </el-col>
        </el-row>

        <el-row :gutter="20" style="margin-top: 30px;">
            <el-col :span="6" style="margin-bottom: 25px;" v-for="(country, index) in countries.data" :key="index">
                <img :src="country?.flags?.png" style="width: 100%; border-radius: 6px;     height: 56%; object-fit: cover;"
                    @click="dialogVisible = true" />
                <h1 class="cut-text">{{ country?.name?.official }} </h1>
                <p>{{ country?.cca2 }}</p>
                <p>{{ country?.cca3 }}</p>
                <el-button type="primary" style="width: 100%; margin-top: 15px;" @click="dialogVisible = true">View
                    Details</el-button>
            </el-col>
        </el-row>
        <el-dialog v-model="dialogVisible" title="" width="500" :show-close="false" :before-close="handleClose">
            <img src="https://flagcdn.com/w320/lu.png"
                style="width: 100%; border-radius: 6px;     height: 56%; object-fit: cover;" />
            <h1>Iceland</h1>
            <p>CCA2: IS</p>
            <p>CCA3: ISL</p>
            <p>Zho Translation: 冰岛</p>
            <p>Alternative Spellings: IS, Island, Republic of Iceland, Lýðveldið Ísland</p>
            <p>IDD Root: +3</p>
            <p>IDD Suffixes: 54</p>
            <div style="text-align: center;">
                <el-button type="primary" @click="closeModal">Got it, Thanks</el-button>
            </div>
        </el-dialog>

    </div>
</template>
  

<script setup>
import axios from "axios";
import { Search } from '@element-plus/icons-vue'
import { ref, onMounted, watch } from 'vue'
// import { useRoute } from 'vue-router'

const baseURL = 'https://restcountries.com/v3.1'

const countries = ref('')
const dialogVisible = ref(false)
const keywords = ref('')
// const route = useRoute() // 2
// const currentCountry = ref(null) // 3
const closeModal = () => {
    dialogVisible.value = false
}
const getCountry = async () => {
    const data = await axios.get(`${baseURL}/all`);
    countries.value = data
}

// 4
// const unwatch = watch(() => route.params.id, async (newId, oldId) => {
//     const result = await axios.get(`${baseURL}/${newId}`)
//     currentCountry.value = result.data

//     // run only once
//     unwatch()
// })


const searchCountry = async () => {
    const countryName = keywords ? keywords : ''
    const data = await axios.get(`${baseURL}/name/${countryName}`);
    countries.value = data
    console.log(countries.value, 'name');
}


onMounted(() => {
    getCountry()
    searchCountry()
})
</script>
<style lang="css">
.cut-text {
    text-overflow: ellipsis;
    overflow: hidden;
    width: 225px;
    height: 1.2em;
    white-space: nowrap;
}
</style>
