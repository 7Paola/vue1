<script setup lang="ts">
import { reactive, type Ref, ref, computed, onMounted, watch } from 'vue'

// 2
const counter = reactive({ count: 0 })
const message = ref('Hello World!')
// 3
const titleClass = ref('title')
// 4 
const count = ref(0)
function increment() {
    count.value++
}

// 5
const text = ref('')
const text2 = ref('')
function onInput(e: any) {
    text.value = e.target.value
}
// 6
const awesome = ref(true)

function toggle() {
    awesome.value = !awesome.value
}
// 7  assegna un ID univoco a ciascun todo
let id = 0

const newTodo = ref('')

type todoType = {
    id: number;
    text: string;
}

const todos: Ref<todoType[]> = ref([
    { id: id++, text: 'Learn HTML' },
    { id: id++, text: 'Learn JavaScript' },
    { id: id++, text: 'Learn Vue' }
])

function addTodo() {
    todos.value.push({ id: id++, text: newTodo.value })
    newTodo.value = ''
}

function removeTodo(todo: todoType) {
    todos.value = todos.value.filter((t) => t !== todo)
}

// 8

const newTodo2 = ref('')
const hideCompleted = ref(false)

type todoType2 = { id: number; text: string; done: boolean }

const todos2 = ref([
    { id: id++, text: 'Learn HTML', done: true },
    { id: id++, text: 'Learn JavaScript', done: true },
    { id: id++, text: 'Learn Vue', done: false }
])
const filteredTodos = computed(() => {
    return hideCompleted.value
        ? todos2.value.filter((t) => !t.done)
        : todos2.value
})


function addTodo2() {
    todos2.value.push({ id: id++, text: newTodo2.value, done: false })
    newTodo2.value = ''
}

const addTodo3 = () => {
    todos2.value.push({ id: id++, text: newTodo2.value, done: false })
    newTodo2.value = ''
}


function removeTodo2(todo2: todoType2) {
    todos2.value = todos2.value.filter((t) => t !== todo2)
}

// 9

const pElementRef = ref<HTMLElement | null>(null)
const piedediporco = () => {
    pElementRef.value && (pElementRef.value.textContent = 'pippo')
}

const piedediporco2 = () => {
    if (pElementRef.value)
        pElementRef.value.textContent = 'pappo'
}

// onMounted(() => {
//     pElementRef.value && (pElementRef.value.textContent = 'pippo')
// })

onMounted(() => {
    piedediporco2
})

// 10

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

watch(todoId, fetchData)

// 10 bis

watch(count, (newCount) => {
    // si, console.log() è un side effect
    console.log(`il nuovo valore di count è: ${newCount}`)
})

//10 ter

const stringaStorta = ref("torta")
watch(stringaStorta, (newStringaStorta) => {
    // si, console.log() è un side effect
    console.log(`il nuovo valore di count è: ${newStringaStorta}`)
})


// 11

import ChildComp from './ChildComp.vue'


// 12

import ChildComp2 from './ChildComp2.vue'
const greeting = ref('Ciao dal padre')

// 13

import ChildComp3 from './ChildComp3.vue'
const childMsg = ref('Ancora nessun messaggio dal figlio')

// 14

import ChildComp4 from './ChildComp4.vue'

const msg = ref('dal padre')

// 15

// import JSConfetti from 'js-confetti'

// const confetti = new JSConfetti()

// function showConfetti() {
//   confetti.addConfetti()
// }

// showConfetti()

</script>

<template>
    <div>

        <h1>Perchè</h1>
        <h2>2</h2>
        <h1>{{ message }}</h1>
        <p>Count is: {{ counter.count }}</p>
        <!-- 3 -->
        <h2>3</h2>
        <h1 :class="titleClass">Rendimi rosso</h1>
        <!-- 4 -->
        <h2>4</h2>
        <button @click="increment">conteggio: {{ count }}</button>
        <!-- 5 -->
        <h2>5</h2>
        <input :value="text" @input="onInput" placeholder="Type here">
        <p>{{ text }}</p>

        <input v-model="text2" placeholder="Scrivi qui">
        <p>{{ text2 }}</p>

        <h2>6</h2>
        <button @click="toggle">toggle</button>

        <h1 v-if="awesome">Vue è fantastico!</h1>
        <h1 v-else>Oh no 😢</h1>


        <h2>7</h2>

        <form @submit.prevent="addTodo">
            <input v-model="newTodo">
            <button>Aggiungi un todo</button>
        </form>
        <ul>
            <li v-for="todo in todos" :key="todo.id">
                {{ todo.text }}
                <button @click="removeTodo(todo)">X</button>
            </li>
        </ul>

        <h2>8</h2>

        <form @submit.prevent="addTodo2">
            <input v-model="newTodo2">
            <button>Aggiungi Todo</button>
        </form>
        <ul>
            <li v-for="todo in filteredTodos" :key="todo.id">
                <input type="checkbox" v-model="todo.done">
                <span :class="{ done: todo.done }">{{ todo.text }}</span>
                <button @click="removeTodo2(todo)">X</button>
            </li>
        </ul>
        <button @click="hideCompleted = !hideCompleted">
            {{ hideCompleted ? 'Mostra tutti' : 'Nascondi completati' }}
        </button>



        <h2>9</h2>

        <p ref="pElementRef">ciao</p>

        <h2>10</h2>

        <p>Todo id: {{ todoId }}</p>
        <button @click="todoId++">Fetch del todo successivo</button>
        <p v-if="!todoData">Caricamento in corso...</p>
        <pre v-else>{{ todoData }}</pre>

        <h2>10 Bis</h2>
        <button @click="count++">patate al forno</button>


        <h2>10 ter</h2>
        <button @click="stringaStorta += 'panna'">torta contorta</button>


        <h2>11</h2>
        <ChildComp />


        <h2>12</h2>
        <ChildComp2 :msg="greeting" />


        <h2>13</h2>
        <ChildComp3 @response="(msg: string) => childMsg = msg" />
        <p>{{ childMsg }}</p>

        <h2>14</h2>
        <ChildComp4>Messaggio: {{ msg }}</ChildComp4>

        <h2>15</h2>
        <h1 @click="showConfetti">🎉 Congratulations!</h1>

    </div>
</template>

<style scoped>
.title {
    color: red;
}

.done {
    text-decoration: line-through;
}

/* h1 {
  text-align: center;
  cursor: pointer;
  margin-top: 3em;
} */
</style>