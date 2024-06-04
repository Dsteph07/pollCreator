<template>
    <div>
        <head> <h1>"Title"</h1></head>
        <div class="list" v-for="(option, index) in Options" :key="index">
        <input type="checkbox" id="checkbox">
        <label for="checkbox">Array.optionTitle</label>
        Options.votes <br>
        </div>
        
    </div>
</template>

<script setup lang="ts">

const props=defineProps<{
    Title: String,
    TotalVotes:Number,
    Options:Array<{optionTitle:String, votes: Number }>
}>()
</script>