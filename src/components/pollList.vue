<template>
    <div>
        <div v-for="(p,index) in polls" :key="index">
            <poll v-model:poll="polls[index]" :index=index @count-new-vote="countNewVote" @delete-poll="deletePoll" @confirm-edits="confirEdits" class="poll-div"></poll>
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
    options:Array<{description:string, choiceId:number, votes:number, color:string}>
}


const props= defineProps<{
    polls:pollT
}>();

const emits=defineEmits(['countNewVote', 'deletePoll', 'confirmEdits']);

function countNewVote(pollId: number, choiceId: number){
    emits('countNewVote', pollId, choiceId);
}

function deletePoll(poll:pollT){
    emits('deletePoll', poll);
}

function confirEdits(newOptions:String[]){
    //console.log(newOptions.value[0]);
    emits('confirmEdits',newOptions);
}



</script>

<style>
#poll-div{
    background-color: red;
}
</style>