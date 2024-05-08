<script setup lang="ts">
import { h } from 'vue';

// // Function to check if a string is a URL
// function isURL(value: string) {
//     const pattern = /^(?:\w+:)?\/\/([^\s.]+\.\S{2}|localhost[:?\d]*)\S*$/;
//     return pattern.test(value);
// }

// function renderName(value: string) {
//     if (isURL(value)) {
//         // return `<a href="${value}">${value}</a>`;
//         // return { tag: 'a', href: value, text: value };
//         // return h('a', { href: value, target: '_blank' }, value);
//         // return h('a', { href: value }, value);
//         // return () => h('div', { id: 'foo' }, 'hello');
//     } else {
//         return () => value;
//     }
// }

function renderName2() {
    return h('div', { id: 'foo' }, 'hello Paolexa')
}

function Hello() {
    return 'hello world!'
}

</script>

<template>
    <h1>Test</h1>
    <p>Prova</p>
    <!-- {{ renderName('https://rickandmortyapi.com/api/episode/1') }}
    <br>
    {{ renderName('sdfsdf sdsdgsd') }} -->

    <br>

    {{ Hello() }}
    <!-- {{ h('div', { id: 'foo' }, 'hello ABCDE') }} -->

    {{ renderName2() }}

    <p>Prova - FINE</p>
</template>