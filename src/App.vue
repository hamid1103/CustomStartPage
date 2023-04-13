<script setup>

import Searchbar from "./components/searchbar.vue";
import Shortcuts from "./components/shortcuts.vue";
import Newscutmodal from "./components/newscutmodal.vue";
import SettingsModal from "./components/settingsModal.vue";
</script>

<template>
    <div id="main">

        <div id="DateTime">
            <p>{{time}}</p>
            <p>{{date}}</p>
        </div>
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
            time: '',
            date: ''
        }
    },
    created() {
        setInterval(this.getnow, 1000);
    },
    mounted() {
        this.emitter.on('addcutmodal', ()=>{
            this.newscut = !this.newscut
            console.log('Set mod')
        })
        this.emitter.on('closeaddcut', ()=>{
            this.newscut = false
            console.log('closing')
        })
        this.emitter.on('showsetmodal', ()=>{
            this.settingsmod = true
        })
        this.emitter.on('closesetmodal', ()=>{
            this.settingsmod = false
        })
    },
    methods: {
        getnow(){
            const today = new Date();
            const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
            const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
            this.date = date;
            this.time = time;
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
    min-height: 100vh;
}
#DateTime{
    height: 2em;
    width: 30em;
    display: flex;
    margin: 1em;
    justify-content: space-between;
}
</style>
