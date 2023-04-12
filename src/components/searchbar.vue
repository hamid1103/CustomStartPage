<template>

    <div id="searchbar">

        <div id="selector">
            <img v-if="choice === 0" src="../assets/ddg.png" alt="Duck Duck Go" />
            <img v-else src="../assets/goog.png" alt="Google">
        </div>

        <form id="searchform" method="get" :action="link">
            <input type="text" name="q">
        </form>

    </div>

</template>

<script>
export default {
    name: "searchbar",
    data() {
        return {
            choice: 0,
            seldiv: document.getElementById('selector'),
            lastScrollTop: 0,
            link: "https://www.google.com/search"
        }
    },
    created () {
        window.addEventListener('click', this.handleC);
    },
    unmounted() {
        window.removeEventListener('click', this.handleC);
    },
    methods: {
        handleC (e){
            if(e.target.id === 'selector' || e.target.parentNode.id === 'selector'){
                console.log(e.target)
                if(this.choice === 0 ){
                    this.choice = 1;
                    this.link = 'https://www.google.com/search'
                }else {
                    this.choice = 0
                    this.link = "https://duckduckgo.com/?q="
                }
            }
        }
    }
}
</script>

<style scoped>
@import "@catppuccin/palette/style";

#searchbar{
    justify-content: center;
    display: flex;
    flex-direction: row;
    align-items: center;
}

#searchbar input{
    width: 53em;
    background-color: var(--ctp-mocha-surface2);
    color: var(--ctp-mocha-text);
    font-size: 1em;
    border-radius: 1em;
    height: 2.7em;
    border: 0;
}
#searchbar input:hover{
    border: var(--ctp-mocha-mauve) solid 1px;
}
#searchbar input:focus{
    outline: var(--ctp-mocha-teal) solid 1px;
}

#selector{
    margin-right: 0.79em;
    width: 3em;
    height: 3em;
    //background-color: var(--ctp-mocha-crust);
    background-color: var(--ctp-mocha-lavender);
    border-radius: 0.9em;
}
#selector img{
    width: 3em;
    object-fit: contain;
    height: 3em;
}
</style>