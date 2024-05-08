<script setup lang="ts">
import { h } from 'vue';

function renderName(s: string) {
    return h('div', { id: 'foo' }, 'hello ' + s)
}

function Hello() {
    return 'hello world!'
}

</script>

<template>
    <h1>Test</h1>

    {{ Hello() }}

    <!-- {{ renderName("Test 1") }} -->
</template>