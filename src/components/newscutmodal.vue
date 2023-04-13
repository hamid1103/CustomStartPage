<template>
    <div class="modal-overlay">
        <div class="modal">
            <div id="preview">
                <a class="scut" :href="LinkLink">
                    <div class="scutcard">
                        <img :src="LinkIMG" :alt="LinkIMG">
                    </div>
                    <p>
                        {{ LinkName }}
                    </p>
                </a>
            </div>

            <div id="settings">
                <input type="text" id="name" v-model="LinkName"  placeholder="Name">
                <input type="text" id="link" v-model="LinkLink" placeholder="Link">
                <input type="text" id="imglink" v-model="LinkIMG" placeholder="Image Link">
                <select id="type">

                </select>
            </div>

            <div id="bottomButtons">
                <div id="save" @click="savescut">
                    Save
                </div>
                <div id="close" @click="close">
                    Close
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "newscutmodal",
    methods: {
        savescut() {
            //get old scuts
            let data = JSON.parse(localStorage.getItem('scuts'))
            //get new scut data
            const linkName = document.getElementById('name').value
            const linkLink = document.getElementById('link').value
            const linkImg = document.getElementById('imglink').value
            const linkType = document.getElementById('type').value

            //write to local storage
            data.tags[linkType].push({
                name: linkName,
                iconLink: linkImg,
                link: linkLink
            })

            localStorage.setItem('scuts', JSON.stringify(data))

            //send update
            this.emitter.emit('updatescuts')

            //close modal
            this.close();
        },
        close() {
            this.emitter.emit('closeaddcut')
        }
    },
    data() {
        return {
            LinkName: '',
            LinkIMG: '',
            LinkLink: '',
        }
    },
    mounted() {
        let data = JSON.parse(localStorage.getItem('scuts'))
        let select = document.getElementById('type')
        //add tags in selection form
        for (let tag in data.tags) {
            let newOption = document.createElement('option')
            newOption.value = tag
            newOption.innerHTML = tag
            select.appendChild(newOption)
        }
    }
}
</script>

<style scoped>
@import "@catppuccin/palette/style";

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

#preview{
    display: flex;
    justify-content: center;
}

#settings {
    display: flex;
    flex-direction: column;
}

#settings input, select {
    margin-top: 0.7em;
    background-color: var(--ctp-mocha-overlay0);
    color: var(--ctp-mocha-text);
    font-size: 1em;
    border-radius: 0.5em;
}

.modal-overlay {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: center;
    background-color: #000000da;
}

.modal {
    text-align: center;
    background-color: white;
    height: 500px;
    width: 500px;
    margin-top: 10%;
    padding: 1em;
    border-radius: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

#bottomButtons {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
}

#close {
    background-color: var(--ctp-mocha-red);
    width: 15vw;
    min-height: 3vh;
    border-radius: 1em;
    font-size: 2em;
    color: white;
}

#save {
    color: white;
    font-size: 2em;
    background-color: var(--ctp-mocha-sky);
    width: 15vw;
    min-height: 3vh;
    border-radius: 1em;
}

#save:hover, #close:hover {
    background-color: var(--ctp-mocha-mauve);
    border: var(--ctp-mocha-lavender) 1px solid;
}

.close-img {
    width: 25px;
}

.check {
    width: 150px;
}

h6 {
    font-weight: 500;
    font-size: 28px;
    margin: 20px 0;
}

p {
    font-size: 16px;
    margin: 20px 0;
}

button {
    background-color: var(--ctp-mocha-sky);
    width: 150px;
    height: 40px;
    color: white;
    font-size: 14px;
    border-radius: 16px;
    margin-top: 50px;
}
</style>