<script setup>

import Searchbar from "./components/searchbar.vue";
</script>

<template>
    <div id="main">
        <searchbar></searchbar>
    </div>
</template>

<style scoped>
#main{
    display: flex;
    flex-direction: column;
    justify-items: center;
    justify-content: center;
    align-items: center;
    align-content: center;
    width: 100vw;
    height: 100vh;
}
</style>
