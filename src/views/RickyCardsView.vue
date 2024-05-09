<script setup lang="ts">
import RickyCard from "@/components/RickyCard.vue";
import { ref, watch } from "vue";
import { type RickAndMortyData } from "../components/ApiRicky.vue"

const page = ref<number>(1)
const maxPage = ref<number>(1)
const pageData = ref<RickAndMortyData>()

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

    pageData.value = await res.json()
    console.log('Qualcosa', pageData.value?.info)
    console.log('Qualcosa2', pageData.value!.info)
    console.log('Qualcosa3', pageData.value?.info.next)
    console.log('Pagine', pageData.value?.info.pages)

    maxPage.value = pageData.value?.info.pages || 1

}

fetchData()
watch(page, fetchData)

function changePage() {
    // Ensure page is within the minimum and maximum values
    if (page.value < 1) {
        page.value = 1;
    } else if (page.value > pageData.value!.info.pages) {
        page.value = pageData.value!.info.pages;
    }
}


</script>

<template>
    <h1>Cards</h1>

    <p>Page: {{ page }} of {{ pageData?.info.pages }}</p>
    <!-- <button v-if="1 < page" @click="page--">Page--</button> -->
    <button :disabled="!(page > 1)" @click="page--">Page--</button>
    <button :disabled="!(page < maxPage)" @click="page++">Page++</button>
    <input type="number" v-model.number="page" :max="maxPage" @input="changePage">

    <div class="cards-container">
        <!-- <RickyCard :id="1" :name="'Pippo'" :url="'https://rickandmortyapi.com/api/character/avatar/1.jpeg'"></RickyCard> -->
        <RickyCard v-for="character in pageData?.results" :key="character.id" :id="character.id" :name="character.name"
            :url="character.image">
        </RickyCard>
    </div>
</template>

<style scoped>
.cards-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;

}
</style>