<template>
  <header> <h1>Poll Creator</h1></header>
  <div class="main">
    <div class='formulaire'>
      <pollForm  @addPoll="addPoll" v-model:id="pollCount"></pollForm>
    </div>

    <div class="polls">
      <pollList v-model:polls="polls" @delete-poll="deletePoll" @count-new-vote="countNewVote"></pollList>
    </div>

  </div>
</template>

<script setup lang="ts">
import {ref, watch, onMounted} from 'vue';
import pollForm from './components/PollForm.vue';
import pollList from './components/pollList.vue';

const polls=ref([]);
let pollCount=polls.value.length;
type poll={
  title:string,
  numberOfVotes:number,
  numberOfOptions:number,
  options:Array<{description:String, choiceId:number, votes:number, color:string}>
}

function addPoll(poll:poll){
  polls.value.push(poll);
  pollCount+=1;
}

function deletePoll(poll:poll){
  polls.value=polls.value.filter(p=> p!=poll)
  pollCount--;
}

function countNewVote(pollIndex:number, choiceId:number){
  if(choiceId!=null){
    polls.value[pollIndex].options[choiceId].votes++;
    polls.value[pollIndex].numberOfVotes++;
  }

  //alert("Thank you for your contribution, your vote has been registered")
}



watch(polls, newVal =>{
    localStorage.setItem('polls', JSON.stringify(newVal));
    localStorage.setItem('pollCounter', JSON.stringify(pollCount));
    },{deep:true})

onMounted(() =>{
      polls.value = JSON.parse(localStorage.getItem('polls') || '[]');
      pollCount= JSON.parse(localStorage.getItem('pollCounter') || '0')
    })


</script>



<style scoped>
header {
  position:absolute;
  top:2%;
  left:30%;
  line-height: 1.5;
}

.main{
  display: flex;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.formulaire{
  position: absolute;
  top: 2%;
  left: 2%;
}

.polls{
  position:absolute;
  top:30%;
  left: 2%;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
