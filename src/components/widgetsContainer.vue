<template>
    <div id="widgetCont">
        <div id="widgetHeader">
            <div id="Mail" class="widgetab">
                <span class="material-symbols-outlined">mail</span>
            </div>
            <div id="News" class="widgetab">
                <span class="material-symbols-outlined">newspaper</span>
            </div>
            <div id="Stock" class="widgetab">
                <span class="material-symbols-outlined">monitoring</span>
            </div>
        </div>
        <div id="widgethold">
            <mail v-if="currentWidget == 'Mail'"></mail>
            <news v-else-if="currentWidget == 'News'"></news>
        </div>
    </div>

</template>

<script>
import Mail from "./Mail.vue";
import News from "./News.vue";

export default {
    name: "widgetsContainer",
    components: {News, Mail},
    mounted() {
        document.getElementById('widgetHeader').addEventListener('click', this.setWidget)
    },
    data(){
        return{
            currentWidget: 'Mail',

        }
    },
    computed: {
    },
    methods: {
        setWidget(e){
            let id;
            if(e.target.className === "widgetab"){
                id = e.target.id
            }else if(e.target.parentNode.className === "widgetab"){
                id = e.target.parentNode.id
            }
            this.currentWidget = id;
            console.log(this.currentWidget)
        },
        renderWidget(){
            let widgetholder = document.getElementById('widgethold')
        }
    }
}
</script>

<style scoped>
@import "@catppuccin/palette/style";
#widgetCont{
    display: flex;
    flex-direction: column;
}
#widgetHeader{
    display: flex;
    flex-direction: row;
    width: 56em;
    height: 2em;
    background-color: var(--ctp-mocha-surface2);
    border-radius: 1em 1em 0 0;
}
#widgetHeader .widgetab{
    height: 2em;
    width: 3em;
    margin-left: 1px;
    background-color: var(--ctp-mocha-overlay2);
    border-radius: 1em 1em 0 0;
    justify-content: center;
}
#widgetHeader .widgetab:hover{
    background-color: var(--ctp-mocha-overlay0);
}
#widgetHeader .widgetab img{
    height: 1.5em;
    object-fit: contain;
    width: 1.5em;
}
#widgethold{
    width: 56em;
    border-radius: 0 0 1em 1em;
    min-height: 5em;
    background-color: var(--ctp-mocha-surface0);
}
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>