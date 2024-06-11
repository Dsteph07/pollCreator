<template>
    <div v-if="!edit" class="poll-no-edit">
        {{ poll.title }}
        <div v-for="(option, index) in poll.options" :key="index" 
        class="option"
        :style=" showResults ? {width: getVotePercentage(option.votes)+ '%', backgroundColor:option.color} : {}"
        >
                <div class="option-name"> {{option.description}}  </div>
                <input type="radio" :value="option" v-model="choice"> 
                <div class="votesPercentage">{{getVotePercentage(option.votes)}} %</div>
        </div>
        <button type="submit" @click="countNewVote"> Submit Vote </button>
        <button @click="deletePoll"> Delete Poll</button>
        <button @click="toggleResults"> {{ showResults ? "Hide" : "Show"}} Results</button>
        <button @click="toggleEdit"> Edit poll </button>
        <span v-if="showResults"> Total Votes: {{ poll.numberOfVotes }}</span>
    </div>

    <div v-if="edit" class="poll-edit">
        <input type="text"  v-model="newOptionDesc">
        <button @click="addNewOption"> Add choice</button><br>
        <label for="newOptions"> Options to be added: </label><br>
        <div  v-for="(option,index) in newOptions" :key="index" name="newOptions"> 
            <input type="text" v-model="newOptions[index]">
        </div>

        <button @click="discardEdits">Discard edits</button>
        <button @click="confirmEdits"> Submit edits</button>
    </div>
    
</template>

<script setup lang="ts">
    import type { RefSymbol } from '@vue/reactivity';
import { defineProps, defineEmits, ref } from 'vue';

    type option={
        description:string,
        optionId:number,
        votes:number,
        color:string
    }

    type pollT={
        title:string;
        numberOfVotes:number;
        numberOfOptions:number;
        options:Array<option>
    }

    const emits=defineEmits(['countNewVote', 'deletePoll', 'confirmEdits' ]);



    const props = defineProps<{
            poll:pollT,
            index:number,
            length:number,
        }>()

    const colors=["red", "blue", "yellow", "purple","green", "pink","cyan"];


    const choice=ref({});
    const showResults=ref(false);
    const newOptionDesc=ref('');
    const edit=ref(false);
    const newOptions=ref([]);
    const confirmedEdits=ref([]);

    function countNewVote(){
        console.log(choice);
        emits('countNewVote', props.index, choice.value.optionId)
    }

    function deletePoll(){
        emits('deletePoll',props.poll);
        choice.value={};
    }

    function discardEdits(){
        toggleEdit();
        newOptions.value=[];
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

    function addNewOption(){
        newOptions.value.push(newOptionDesc.value);
    }
    function confirmEdits(){
        confirmedEdits.value=props.poll;
        for(let i=0; i< newOptions.value.length;i++){
            const newOption:option={
                description:newOptions.value[i],
                optionId:confirmedEdits.value.numberOfOptions,
                votes:0,
                color:colors[(props.poll.numberOfVotes+i)%colors.length]
            }
            confirmedEdits.value.options.push(newOption);
            confirmedEdits.value.numberOfOptions++;
        }
    toggleEdit();
    }

    function deleteOption(option){

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