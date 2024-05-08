<script setup lang="ts">
// defineProps<{
//   msg: string
// }>()

import { ref, reactive, watch } from "vue"

const counter = reactive({ count: 0, pippo: 3, turi: { pluto: 66 } })
const message = ref('Hello World!')
message.value = message.value + " Pluto"
// Col ref ci vuole il value
console.log("Message:", message.value)
console.log("Message:", message)
// Col reactive no ma specificare la chiave
counter.count = 5
console.log("Counter fdhgds .count:", counter.count)
console.log("Counter.pippo:", counter.pippo)
console.log("Counter.turi:", counter.turi)
console.log("Counter.turi.pluto:", counter.turi.pluto)

const a = ref(true)
// a.value = false

const b = reactive({ stampo: true })
b.stampo = false

const vettore = ["A", "Pippo", "Pluto", 4, 5, 6, 7, 8]
const ogg = { a: "A", b: "Pippo", c: "Pluto" }

const colorClass = ref("redClass")
colorClass.value = "blueClass"

function f1() {
    alert("Hai cliccato")
}

const text = ref('')

let id = 0
const todos = ref([
    { id: id++, text: 'Learn HTML' },
    { id: id++, text: 'Learn JavaScript' },
    { id: id++, text: 'Learn Vue' }
])
const newTodo = ref('')
function addTodo() {
    todos.value.push({ id: id++, text: newTodo.value })
    newTodo.value = ''
}

const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
    todoData.value = null
    const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
    )
    todoData.value = await res.json()
}
fetchData()
watch(todoId, fetchData, { immediate: true, once: false })

</script>

<template>
    <div>
        <h1>Prova</h1>

        <h1>{{ message }}</h1>
        <p>Count is: {{ counter.count }}</p>
        <p>Pippo is: {{ counter.pippo }}</p>

        <h2 v-if="a">Koala {{ a }}</h2>
        <h2 v-if="!a">Paola {{ a }}</h2>
        <h2 v-if="b.stampo">Koala reactive {{ b.stampo }}</h2>
        <!-- <h2 v-if="!b">Paola reactive {{ b.stampo }}</h2> -->
        <h2 v-if="!b.stampo">Koala reactive {{ b.stampo }}</h2>

        <ul>
            <li v-for="elm in vettore">{{ elm }}</li>
        </ul>

        <h4>V for oggetto</h4>
        <ul>
            <li v-for="(e, k) in ogg">{{ k }} - {{ e }}</li>
        </ul>

        <h2 :class="colorClass">Sono rosso</h2>
        <h2 v-bind:class="colorClass">Sono rosso</h2>
        <h2 v-bind:id="'Pippo'">Sono rosso</h2>

        <button @click="f1">Cliccami</button>

        <br>
        <input v-model="text" placeholder="Scrivi qui">
        <p>{{ text }}</p>


        <form @submit.prevent="addTodo">
            <input v-model="newTodo">
            <p>{{ newTodo }}</p>
            <button>Aggiungi un todo</button>
        </form>

        <ul>
            <li v-for="todo in todos" :key="todo.id">
                {{ todo.text }}
                <!-- <button @click="removeTodo(todo)">X</button> -->
            </li>
        </ul>


        <p>Todo id: {{ todoId }}</p>
        <button @click="todoId++">Fetch del todo successivo</button>
        <p v-if="!todoData">Caricamento in corso...</p>
        <pre v-else>{{ todoData }}</pre>

    </div>
</template>

<style scoped>
.redClass {
    color: red
}

.blueClass {
    color: blue
}
</style>