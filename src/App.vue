<script setup>

import Searchbar from "./components/searchbar.vue";
import Shortcuts from "./components/shortcuts.vue";
import Newscutmodal from "./components/newscutmodal.vue";
import SettingsModal from "./components/settingsModal.vue";
</script>

<template>
    <div id="main">
        <searchbar></searchbar>
        <shortcuts></shortcuts>
        <newscutmodal v-show="newscut"></newscutmodal>
        <settings-modal v-show="settingsmod"></settings-modal>
    </div>
</template>

<script>
export default {
    data(){
        return{
            newscut: false,
            settingsmod: false,
        }
    }
}
</script>

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
