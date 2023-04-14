<template>
    <p>Mail</p>
</template>

<script>
import ImapClient from 'emailjs-imap-client'
//var client = new ImapClient(host[, port][, options])
export default {
    name: "Mail",
    settingsDone: localStorage.getItem('emailset')
}
</script>

<style scoped>

</style>