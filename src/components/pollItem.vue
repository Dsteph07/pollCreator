<template>
    <div v-if="!edit" class="poll-no-edit">
        {{ poll.title }}
        <div v-for="(option, index) in poll.options" :key="index" 
        class="option"
        :style=" showResults ? {width: getVotePercentage(option.votes)+ '%', backgroundColor:option.color} : {}"
        >
                <div class="option-name"> {{option.description}}  </div>
                <input type="radio" name="choice" v-model="choice"> 
                <div class="votesPercentage">{{getVotePercentage(option.votes)}} %</div>
        </div>
        <button type="submit" @click="countNewVote"> Submit Vote </button>
        <button @click="deletePoll"> Delete Poll</button>
        <button @click="toggleResults"> {{ showResults ? "Hide" : "Show"}} Results</button>
        <button @click="toggleEdit"> Edit poll </button>
        <span v-if="showResults"> Total Votes: {{ poll.numberOfVotes }}</span>
    </div>

    <div v-if="edit" class="poll-edit">
        <input type="text" v-model="editedPoll.title">
        <div v-for="(option, index) in editedPoll.options" :key="index" >
                <input v-model="option.description">
        </div>
        <button @click="toggleEdit">Discard edits</button>
        <button> Submit edits</button>
    </div>
    
</template>

<script setup lang="ts">
    import { defineProps, defineEmits, ref } from 'vue';



    type pollT={
        title:string;
        numberOfVotes:number;
        numberOfOptions:number;
        options:Array<{description:string, choiceId:number, votes:number, color:string}>
    }

    const emits=defineEmits(['countNewVote', 'deletePoll']);



    const props = defineProps<{
            poll:pollT,
            index:number,
        }>()


    const choice=ref({});
    const showResults=ref(false);
    const edit=ref(false);
    const editedPoll=ref({});
    editedPoll.value=props.poll;
    console.log(editedPoll);

    function countNewVote(){
        emits('countNewVote', props.index, choice.value.optionId)
    }

    function deletePoll(){
        emits('deletePoll',props.poll);
        choice.value={};
    }

    function toggleResults(){
        showResults.value=!showResults.value;
    }

    function toggleEdit(){
        edit.value=!edit.value;
    }

    function getVotePercentage(votes:number):string{
        const totalVotes= props.poll.numberOfVotes;
        return totalVotes> 0 ? ((votes / totalVotes) * 100).toFixed(2) : 0;
    }
</script>

<style>

.option{
}

.result-bar{

}

.poll-no-edit{
    border:2px solid black;
}

</style>