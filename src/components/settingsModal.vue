<template>
    <div class="modal-overlay">
        <div class="modal">
            <div v-show="hasalert" id="alert">
                <p>{{alert}}</p>
            </div>
            <div id="pinSettings">
                <h3>Private pin settings</h3>
                <form id="pinSetForm" @submit="setpin">
                    <input id="pinOld" type="password" pattern="[0-9]+" placeholder="Current Pin">
                    <input id="pinNew" type="password" pattern="[0-9]+" placeholder="New Pin">
                    <input id="pinConfirm" type="password" pattern="[0-9]+" placeholder="Confirm Pin">
                    <input type="submit" value="Set pin">
                </form>
            </div>

            <div id="bottomButtons">
                <div id="save" @click="savesets">
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
    name: "settingsModal",
    methods: {
        savesets(){
        },
        close(){
            this.emitter.emit('closesetmodal')
        },
        setpin(e){
            e.preventDefault()
            console.log('submitted pin form')
            let oldpin = document.getElementById('pinOld').value
            let newpin = document.getElementById('pinNew').value
            let pinconf = document.getElementById('pinConfirm').value
            //check if old pin is correct
            if(oldpin === localStorage.getItem('pincode')){
                //check if newpin and confirm are the same
                if (newpin === pinconf){
                    localStorage.setItem('pincode', newpin);
                    this.alert = null;
                }else {
                    //new pins do not match
                    this.alert = 'New pin and confirm pin do not match'
                }
            }else {
                //oldpin does not match
                this.alert = 'Current pin is incorrect'
            }

        }
    },
    computed: {
        hasalert(){
            return this.alert != null;
        }
    },
    data(){
        return{
            alert: null
        }
    }
}
</script>

<style scoped>
@import "@catppuccin/palette/style";
#alert{
    background-color: var(--ctp-mocha-mauve);
    min-height: 2em;
}

#pinSetForm{
    display: flex;
    flex-direction: column;
    align-items: center;
}
#pinSetForm input{
    border-radius: 0.5em;
    width: 7em;
    margin-bottom: 0.3vh;
    color: var(--ctp-mocha-text)
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
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 500px;
    width: 500px;
    margin-top: 10%;
    padding: 60px 0;
    border-radius: 20px;
}
.close {
    margin: 10% 0 0 16px;
    cursor: pointer;
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