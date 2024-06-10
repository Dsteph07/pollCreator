<template>
    <div>
        <input type="text" v-model="tit"><br>
        <input type="text" v-model="option">
        <button id="addOption" @click="addOption(option)">Add option to poll</button><br>
        <label for="options">Your options: </label>
        <div v-for="(opt, index) in options" :key="index" name="options">
            <input type="text" v-model="options[index].description">
        </div><br>
        <button id="addPoll" @click="addPoll">Add poll to collection</button>
    </div>
</template>

<script setup lang="ts">

import {ref, defineEmits, defineProps} from 'vue';

const emit=defineEmits(['addPoll']);
const colors=["red", "blue", "yellow", "purple","green", "pink","cyan"];

const tit=ref('');
const option=ref('');
let length=0;
const options=ref([]);

const props=defineProps<{
    id:number
}>()



function addOption(){
    const opt={
        description:option.value,
        optionId:length,
        votes:0,
        color:colors[length%7],
    }
    length++;
    options.value.push(opt)
    option.value='';   
}

function addPoll(){
    const p={
        title:tit.value,
        numberOfVotes:0,
        numberOfOptions:length,
        options:options.value,
    }
    emit('addPoll', p);

    options.value=[];
    tit.value='';
    length=0;
    
}
</script>