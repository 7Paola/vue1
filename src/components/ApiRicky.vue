<script setup lang="ts">

import { reactive, type Ref, ref, computed, onMounted, watch } from 'vue'



type Character = {
    id: number;
    name: string;
    status: string;
    species: string;
    type: string;
    gender: string;
    origin: {
        name: string;
        url: string;
    };
    location: {
        name: string;
        url: string;
    };
    image: string;
    episode: string[];
    url: string;
    created: string;
}

type Info = {
    count: number;
    pages: number;
    next: string | null;
    prev: string | null;
}

type RickAndMortyData = {
    info: Info;
    results: Character[];
}


const page = ref(1)
const maxPage = ref(1)
const todoData = ref<RickAndMortyData>()

async function fetchData() {
    // todoData.value = null
    const res = await fetch(
        `https://rickandmortyapi.com/api/character/?page=${page.value}`
    )

    // const resData: RickAndMortyData = await res.json()
    // console.log('res', res)
    // console.log('resData', resData)
    // console.log('resData.info', resData.info)
    // console.log('resData NAME', resData.results[0].name)

    todoData.value = await res.json()
    console.log('Qualcosa', todoData.value?.info)
    console.log('Qualcosa2', todoData.value!.info)
    console.log('Qualcosa3', todoData.value?.info.next)
    console.log('Pagine', todoData.value?.info.pages)

    maxPage.value = todoData.value?.info.pages || 1

}

fetchData()
watch(page, fetchData)

// onMounted(fetchData)

function changePage() {
    // Ensure page is within the minimum and maximum values
    if (page.value < 1) {
        page.value = 1;
    } else if (page.value > todoData.value!.info.pages) {
        page.value = todoData.value!.info.pages;
    }
}

</script>

<template>

    <div>

        <h2>10 personaggi in cerca d'autore</h2>

        <p>Page: {{ page }} of {{ todoData?.info.pages }}</p>
        <!-- <button v-if="1 < page" @click="page--">Page--</button> -->
        <button :disabled="!(page > 1)" @click="page--">Page--</button>
        <button :disabled="!(page < maxPage)" @click="page++">Page++</button>
        <input type="number" v-model.number="page" :max="maxPage" @input="changePage">

        <p v-if="!todoData">Caricamento in corso...</p>
        <!-- <pre v-else>{{ todoData }}</pre> -->
        <ol :start="1 + 20 * (page - 1)" v-else>
            <li v-for="character in todoData?.results" :key="character.id" class="character-list">
                <h3><a :href="character.url">{{ character.name }}</a></h3>
                <img :src="character.image" alt=""><br>
                <ul>
                    <li v-for="(val, key) in character" :key="key">
                        <template v-if="key != 'episode'">
                            {{ key }} -
                            <template v-if="Array.isArray(val)">
                                <ul>
                                    <li v-for="(item, index) in val" :key="index">{{ item }}</li>
                                </ul>
                            </template>
                            <template v-else>
                                {{ val }}
                            </template>
                        </template>
                        <template v-else>
                            Episodi -
                            <ul>
                                <li v-for="(item, index) in val" :key="index"><a :href="item">{{ item }}</a></li>
                            </ul>
                        </template>
                    </li>
                </ul>
            </li>
        </ol>
    </div>



</template>

<style scoped>
/* CSS */
.character-list {
    margin-bottom: 50px;
    /* Adjust the value as needed */
}
</style>