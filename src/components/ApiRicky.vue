<script setup lang="ts">

import { reactive, type Ref, ref, computed, onMounted, watch } from 'vue'

import { h } from 'vue';

// Function to check if a string is a URL
function isURL(value: string) {
    const pattern = /^(?:\w+:)?\/\/([^\s.]+\.\S{2}|localhost[:?\d]*)\S*$/;
    return pattern.test(value);
}

function renderName(value: string) {
    if (isURL(value)) {
        return h('a', { href: value, target: '_blank' }, value);
    } else {
        return value;
    }
}






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


const todoId = ref(1)
const todoData = ref<RickAndMortyData>()

async function fetchData() {
    // todoData.value = null
    const res = await fetch(
        `https://rickandmortyapi.com/api/character`
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
}

fetchData()

// watch(todoId, fetchData)

// onMounted(fetchData)

</script>

<template>


    <div>
        <p>Prova</p>
        {{ renderName('https://rickandmortyapi.com/api/episode/1') }}
        <p>Prova - FINE</p>

        <h2>10 personaggi in cerca d'autore</h2>

        <p>Todo id: {{ todoId }}</p>
        <button @click="todoId++">Fetch del todo successivo</button>
        <p v-if="!todoData">Caricamento in corso...</p>
        <!-- <pre v-else>{{ todoData }}</pre> -->
        <ol v-else>
            <li v-for="character in todoData?.results" :key="character.id" class="character-list">
                <h3><a :href="character.url">{{ character.name }}</a></h3>
                <img :src="character.image" alt=""><br>
                <ul>
                    <li v-for="(val, key) in character" :key="key">
                        {{ key }} -
                        <template v-if="Array.isArray(val)">
                            <ul>
                                <li v-for="(item, index) in val" :key="index">{{ item }}</li>
                            </ul>
                        </template>
                        <template v-else>
                            {{ val }}
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