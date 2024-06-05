<template>
    <div>
        <div v-for="(p,index) in polls" :key="index">
            <poll v-model:poll="polls[index]" @count-new-vote="countNewVote" @delete-poll="deletePoll"></poll>
            {{ p }}
        </div>
        
    </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import poll from './pollItem.vue'

type pollT={
    title:string;
    numberOfVotes:number;
    numberOfOptions:number
    pollId:number,
    options:Array<{description:string, choiceId:number, votes:number}>
}


const props= defineProps<{
    polls:pollT
}>();

const emits=defineEmits(['countNewVote', 'deletePoll']);

function countNewVote(pollId: number, choiceId: number){
    emits('countNewVote', pollId, choiceId);
}

function deletePoll(poll:pollT){
    emits('deletePoll', poll);
}



</script>

<style></style>