<template>
  <header> <h1>Poll Creator</h1></header>
  <main>
    <div class='formulaire'>
      <pollForm  @addPoll="addPoll"></pollForm>
    </div>

    <div class="polls">
      <pollList v-model:polls="polls"></pollList>
    </div>

  </main>
</template>

<script setup lang="ts">
import {ref, watch, onMounted} from 'vue';
import pollForm from './components/PollForm.vue';
import pollList from './components/poleList.vue';

const polls=ref([]);


type poll={
  title:string,
  numberOfVotes:number,
  options:Array<{description:String, votes:number}>
}

function addPoll(poll:poll){
  polls.value.push(poll);
}

watch(polls, newVal =>{
    localStorage.setItem('polls', JSON.stringify(newVal))
    },{deep:true})

    onMounted(() =>{
    polls.value = JSON.parse(localStorage.getItem('polls') || '[]')
    })

</script>



<style scoped>
header {
  position:absolute;
  top:2%;
  line-height: 1.5;
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
