<script setup>

import Searchbar from "./components/searchbar.vue";
import Shortcuts from "./components/shortcuts.vue";
import Newscutmodal from "./components/newscutmodal.vue";
import SettingsModal from "./components/settingsModal.vue";
import WidgetsContainer from "./components/widgetsContainer.vue";
</script>

<template>
    <div id="main">
        <WidgetsContainer></WidgetsContainer>
        <div id="DateTime">
            <p>{{time}}</p>
            <div id="weather">
                <img :src="weatherimg" :alt="weatherimg">
                <p>{{weatherString}} &#8451;</p>
            </div>
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
            date: '',
            weatherString: '',
            weatherimg: ''
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
        this.setWeather()

    },
    methods: {
        getnow(){
            const today = new Date();
            const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
            const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
            this.date = date;
            this.time = time;
        },
        setWeather(){
            fetch(import.meta.env.VITE_weather_api_link, {
                "method": "GET",
                "headers": {
                }
            })
                .then(response => {
                    if (!response.ok) {
                        throw response;
                    }
                    return response.json();
                }).then(response => {
                    //determine icon
                    let iconDesc = response.currentConditions.icon;
                    let imglink = 'https://github.com/visualcrossing/WeatherIcons/blob/main/PNG/4th%20Set%20-%20Color/' + iconDesc + '.png?raw=true'
                    this.weatherimg = imglink
                    //set Conditions
                    let condition = response.description;
                    //set temperature
                    let temps = response.currentConditions.temp;
                    let weathtotalstring = condition + 'It is ' + temps
                    this.weatherString = weathtotalstring
                    console.log(response);
                })
                .catch(err => {
                    console.error(err);
                });
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
    min-width: 30em;
    display: flex;
    margin: 1em;
    justify-content: space-between;
}
#weather{
    display: flex;
    min-width: 10em;
    justify-content: center;
}
</style>
