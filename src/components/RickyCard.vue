<script setup lang="ts">

const { id, name, url } = defineProps<{ id: number; name: string; url: string }>();

</script>


<template>
    <!-- <div class="card">
        <h2>{{ name }}</h2>
        <img :src="url" alt=""><br> -->
    <!-- <p>{{ id }} - {{ name }}</p> -->
    <!-- <a :href="`https://rickandmortyapi.com/api/character/${id}`">{{ name }} - details</a>
    </div> -->


    <div class="flip-card">
        <div class="flip-card-inner">
            <div class="flip-card-front">
                <h2>{{ name }}</h2>
                <img :src="url" alt=""><br>
                <!-- <p>{{ id }} - {{ name }}</p> -->
                <a :href="`https://rickandmortyapi.com/api/character/${id}`">{{ name }} - details</a>
            </div>
            <div class="flip-card-back">
                <h1>{{ name }}</h1>
                <p>ID {{ id }}</p>
            </div>
        </div>
    </div>

</template>

<style scoped>
.card {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    transition: 0.3s;
    width: 33%;
    border-radius: 5px;
    margin-bottom: 30px;

}

a {
    text-decoration: none;
}

/* The flip card container - set the width and height to whatever you want. We have added the border property to demonstrate that the flip itself goes out of the box on hover (remove perspective if you don't want the 3D effect */
.flip-card {
    background-color: transparent;
    width: 300px;
    height: 400px;
    border: 1px solid #f1f1f1;
    perspective: 1000px;
    /* Remove this if you don't want the 3D effect */
}

/* This container is needed to position the front and back side */
.flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.flip-card:hover .flip-card-inner {
    transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front,
.flip-card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden;
    /* Safari */
    backface-visibility: hidden;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
    background-color: #bbb;
    color: black;
}

/* Style the back side */
.flip-card-back {
    background-color: dodgerblue;
    color: white;
    transform: rotateY(180deg);
}
</style>