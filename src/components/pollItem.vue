<template>
    <div>
        {{ poll.title }}
        <div v-for="(option, index) in poll.options" :key="index">
                <label for="choice"> {{option.description}}  </label>
                <input type="radio" name="choice" :value="option" v-model="choice"><br>
            </div>
            <button type="submit" @click="countNewVote"> Submit Vote </button>
            <button @click="deletePoll"> Delete Poll</button>
    </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, ref } from 'vue';

const choice=ref({});

type pollT={
    title:string;
    numberOfVotes:number;
    numberOfOptions:number;
    pollId:number;
    options:Array<{description:string, choiceId:number, votes:number}>
}

const emits=defineEmits(['countNewVote', 'deletePoll']);



const props = defineProps<{
        poll:pollT
    }>()

function countNewVote(){
    emits('countNewVote', props.poll.pollId, choice.value.optionId)
}

function deletePoll(){
    emits('deletePoll',props.poll);
}
</script>