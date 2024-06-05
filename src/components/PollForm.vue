<template>
    <div>
        <input type="text" v-model="tit"><br>
        <input type="text" v-model="option">
        <button id="addOption" @click="addOption(option)">Add option to poll</button>
        <div v-for="(opt, index) in options" :key="index">
            {{ options[index].description }}<br>
        </div><br>
        <button id="addPoll" @click="addPoll">Add poll to collection</button>
    </div>
</template>

<script setup lang="ts">

import {ref, defineEmits, defineProps} from 'vue';

const emit=defineEmits(['addPoll']);

const tit=ref('');
const option=ref('');
let length=0;
const options=ref([]);

const props=defineProps<{
    id:number
}>()



function addOption(option: String){
    const opt={
        description:option,
        optionId:length,
        votes:0,
    }
    length++;
    options.value.push(opt)
}

function addPoll(){
    const p={
        title:tit,
        numberOfVotes:0,
        numberOfOptions:length,
        pollId:props.id,
        options:options,
    }
    emit('addPoll', p);
}
</script>