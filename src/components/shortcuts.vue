<template>
    <div id="shortcuts">
        <div id="header">
            <div id="filter">
                <p id="all" class="selected">All</p>
                <p id="dev">Dev</p>
                <p id="social">Social</p>
                <p id="school">School</p>
                <p id="linux">Linux</p>
                <p id="music">Music</p>
                <p id="private">Private (0_0,)</p>
            </div>
            <div id="options">
                <h3 @click="showsettings">Settings</h3>
                <h3 @click="showcuts">Add Shortcut</h3>
            </div>
        </div>
        <div id="scutshold">

        </div>
    </div>
</template>

<script>
export default {
    name: "shortcuts",
    mounted() {
        document.getElementById('all').className = 'selected';
        document.getElementById('music').className = '';
        document.getElementById('dev').className = '';
        document.getElementById('social').className = '';
        document.getElementById('school').className = '';
        document.getElementById('linux').className = '';
        document.getElementById('private').className = '';

        document.getElementById('filter').addEventListener('click', this.setFilter)
        //gen scuts key in localstorage if non existent
        if(localStorage.getItem('scuts') === null){
            let newscutsjson = {
                //we do be doing this by tag, to avoid for looping over every entry
                tags: {
                    private: [
                        {
                            name: 'Cornhub',
                            iconLink: 'https://pbs.twimg.com/profile_images/609214434930782209/-0Es6OzU_400x400.jpg',
                            link: 'https://cornhub.website/'
                        }
                    ],
                    linux: [
                        {
                            name: 'Arch Wiki',
                            iconLink: 'https://wiki.installgentoo.com/images/thumb/f/f9/Arch-linux-logo.png/300px-Arch-linux-logo.png',
                            link: 'https://wiki.archlinux.org/'
                        }
                    ],
                    school: [
                        {
                            name: 'Mijn HR',
                            iconLink: 'https://i1.wp.com/www.bluecity.nl/wp-content/uploads/2019/11/Hogeschool-Rotterdam.png?fit=653%2C490&ssl=1',
                            link: 'https://mijn.hr.nl/'
                        }
                    ],
                    social: [
                        {
                            name: 'Reddit',
                            iconLink: 'https://www.iconpacks.net/icons/2/free-reddit-logo-icon-2436-thumb.png',
                            link: 'https://reddit.com'
                        }
                    ],
                    dev: [
                        {
                            name: 'Github',
                            iconLink: 'https://cdn-icons-png.flaticon.com/512/25/25231.png',
                            link: 'https://github.com'
                        }
                    ],
                    music: [
                        {
                            name: 'Ghost Hack',
                            iconLink: 'https://s3-eu-west-1.amazonaws.com/tpd/logos/5cd702fb0fb4a100010dc160/0x0.png',
                            link: 'https://www.ghosthack.de/'
                        },
                        {
                            name: 'Looperman',
                            iconLink: 'https://i0.wp.com/djtechtools.com/wp-content/uploads/2012/05/looperman-logo.png?resize=200%2C200',
                            link: 'https://www.looperman.com'
                        }
                    ]
                }
            }
            localStorage.setItem('scuts', JSON.stringify(newscutsjson))
        }
        //init shortcuts
        this.shortcutstorage = JSON.parse(localStorage.getItem('scuts'))
        //add event for update shortcuts list
        this.emitter.on('updatescuts', ()=>{
            //update local shortcutstorage
            this.shortcutstorage = JSON.parse(localStorage.getItem('scuts'))
            //rerender list
            console.log(this.shortcutstorage)
            this.renderscuts()
        })
        //first render, render all
        this.renderscuts()
    },
    data(){
        return{
            shortcutstorage: null,
            currentfilt: 'all',
        }
    },
    computed:{
        showpin(){
            return this.currentfilt === 'private'
        },
        pincode(){
            if(localStorage.getItem('pincode')){
                return localStorage.getItem('pincode')
            }else {
                //set default pincode
                localStorage.setItem('pincode', '1234')
                return localStorage.getItem('pincode')
            }
        }
    },
    methods:{
        showcuts(){
            this.emitter.emit('addcutmodal')
        },
        showsettings(){
            this.emitter.emit('showsetmodal')
        },
        setFilter(e){
            if(e.target.nodeName === "P"){
                //reset filters
                document.getElementById('all').className = '';
                document.getElementById('music').className = '';
                document.getElementById('dev').className = '';
                document.getElementById('social').className = '';
                document.getElementById('school').className = '';
                document.getElementById('linux').className = '';
                document.getElementById('private').className = '';

                let scutshold = document.getElementById('scutshold')

                e.target.className = 'selected'
                this.currentfilt = e.target.id

                //clear shortcuts render
                scutshold.innerHTML = ''

                //render filtered shortcuts
                    //if id = all, render all
                let tagsdata = this.shortcutstorage.tags
                if(this.currentfilt === 'all'){
                    this.renderscuts();
                }else if(this.currentfilt === 'private'){
                    scutshold.innerHTML = '<form id="placepin" v-if="showpin">\n' +
                        '            <input id="pinfinput" type="password" pattern="[0-9]+" placeholder="Pin">\n' +
                        '            </form>'
                    document.getElementById('placepin').addEventListener('submit', (e)=>{
                        e.preventDefault()
                        let input = document.getElementById('pinfinput')
                        if(input.value === this.pincode){
                            scutshold.innerHTML = ''
                            for(let jscut in tagsdata[this.currentfilt]){
                                let data = tagsdata[this.currentfilt][jscut]
                                let newA = document.createElement('a')
                                newA.className = 'scut'
                                let innerDiv = document.createElement('div')
                                innerDiv.className = 'scutcard'
                                newA.appendChild(innerDiv)
                                let img = document.createElement('img')
                                img.src = data.iconLink
                                newA.href = data.link
                                let newP = document.createElement("p")
                                newP.innerHTML = data.name
                                newA.appendChild(newP)
                                innerDiv.appendChild(img)
                                //.scut .scutcard
                                scutshold.appendChild(newA)
                            }
                        }else{
                            input.value = '';
                            input.placeholder = 'pin incorrect'
                        }

                    })
                }
                else{
                    for(let jscut in tagsdata[this.currentfilt]){
                        let data = tagsdata[this.currentfilt][jscut]
                        let newA = document.createElement('a')
                        newA.className = 'scut'
                        let innerDiv = document.createElement('div')
                        innerDiv.className = 'scutcard'
                        newA.appendChild(innerDiv)
                        let img = document.createElement('img')
                        img.src = data.iconLink
                        newA.href = data.link
                        let newP = document.createElement("p")
                        newP.innerHTML = data.name
                        newA.appendChild(newP)
                        innerDiv.appendChild(img)
                        //.scut .scutcard
                        scutshold.appendChild(newA)
                    }
                }
            }
        },
        renderscuts(){
            let scutsholder = document.getElementById('scutshold')
            scutsholder.innerHTML = ''
            let tagsdata = this.shortcutstorage.tags
            for(let tag in tagsdata){
                if(tag === 'private'){
                }else {
                    for(let jscut in tagsdata[tag]){
                        let data = tagsdata[tag][jscut]
                        let newA = document.createElement('a')
                        newA.className = 'scut'
                        let innerDiv = document.createElement('div')
                        innerDiv.className = 'scutcard'
                        newA.appendChild(innerDiv)
                        let img = document.createElement('img')
                        img.src = data.iconLink
                        newA.href = data.link
                        let newP = document.createElement("p")
                        newP.innerHTML = data.name
                        newA.appendChild(newP)
                        innerDiv.appendChild(img)
                        //.scut .scutcard
                        scutsholder.appendChild(newA)
                    }
                }
            }
        }
    }
}
</script>

<style>
@import "@catppuccin/palette/style";

#pinfinput {
    background-color: var(--ctp-mocha-surface0);
    width: 8em;
    height: 2vh;
    border-radius: 1em;
    border: var(--ctp-mocha-crust) 2px solid;
}
#pinfinput:invalid{
    border: var(--ctp-mocha-red) 2px solid;
}

#scutshold{
    display: flex;
    flex-flow: row wrap;
    width: 56em;
    justify-content: space-evenly;
    min-height: 5em;
    margin-top: 1em;
}
#shortcuts{
    display: flex;
    flex-direction: column;
    margin-top: 2em;
    margin-bottom: 2em;
    width: 56em;
    min-height: 5em;
    background-color: var(--ctp-mocha-surface1);
    border-radius: 1em;
}
#shortcuts #header{
    width: 56em;
    height: 3em;
    border-bottom: var(--ctp-mocha-crust) solid 1px;
    align-content: space-evenly;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
#shortcuts #header #options{
    display: flex;
    flex-direction: row-reverse;
    justify-content: space-between;
}
#shortcuts #header #filter{
    display: flex;
    flex-direction: row-reverse;
    justify-content: space-evenly;
}
#shortcuts #header #filter p{
    margin-left: 0.7em;
    color: var(--ctp-mocha-text);
}
#filter .selected {
    color: var(--ctp-mocha-red);
    text-decoration: underline;
}
#shortcuts #header #filter p:hover{
    color: var(--ctp-mocha-subtext0);
    text-decoration: underline;
}
#shortcuts #header h3{
    color: var(--ctp-mocha-text);
    margin-right: 0.9em;
}
#shortcuts #header h3:hover{
    color: var(--ctp-mocha-subtext0);
    text-decoration: underline;
}

.scut{
    width: 4.5em;
    display: flex;
    flex-direction: column;
}
.scut .scutcard{
    width: 4.5em;
    height: 4.5em;
    border-radius: 1em;
    background-color: var(--ctp-mocha-overlay0);
    border: var(--ctp-mocha-crust) solid 3px;
}
.scutcard img{
    width: 4.4em;
    height: 4.4em;
    border-radius: 1em;
    object-fit: contain;
}
.scut .scutcard:hover{
    border: var(--ctp-mocha-maroon) solid 3px;
}
.scut p{
    margin-top: 0;
}


</style>