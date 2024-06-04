<template>
    <div>
        {{ poll.title }}
    </div>
</template>

<script setup lang="ts">
import { defineProps, ref } from 'vue';

type pollT={
    title:string;
    numberOfVotes:number;
    options:Array<{description:string, votes:number}>
}



const props = defineProps<{
        poll:pollT
    }>()
console.log(props.p);

</script>