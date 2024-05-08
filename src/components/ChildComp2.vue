<script setup>
const props = defineProps({
  msg: String
})
</script>

<template>
  <h2>{{ msg || 'Ancora nessuna props passata' }}</h2>
</template>