<template>
    <div>
        <div v-for="(p,index) in polls" :key="index">
            <poll v-model:poll="polls[index]"></poll>
        </div>
        
    </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';
import poll from './pollItem.vue'


const props= defineProps<{
    polls:Array<{title:string, numberOfVotes:number, options: Array<{description:string, votes:number}>     }>
}>();


</script>