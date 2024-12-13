<script setup>
import { ref } from 'vue';

import Chord from "./Chord.vue";

const props = defineProps({
  chordFamily: Object
})

const emit = defineEmits(['chordClicked']);


const show = ref(true);
const mainClass = ref('nextchord-proposal-family-name')
const faClass  = ref('fa')
const faChevronClass = ref('fa-chevron-down')

function foldUnfold(){
    show.value = !show.value;
    (show.value) ? faChevronClass.value = 'fa-chevron-down' : faChevronClass.value = 'fa-chevron-right'
}



</script>


<template>
    <div @click="foldUnfold" :class="[mainClass, faClass, faChevronClass]">&nbsp;{{ chordFamily.familyName || 'NA' }}</div>
    <Transition name="familyfold">
        <div v-if="show"  class="nextchord-proposal-family">
            <div class="separator">&nbsp;</div>
            <div class="nextchord-proposal-family-list">
                <Chord 
                  v-for="chord in chordFamily.chords" 
                  v-bind:chord="chord" 
                  v-on:chordClicked="$emit('chordClicked',chord,chordFamily.scaleName,chordFamily.scaleTonic)"
                />
            </div>
        </div>
    </Transition>

</template>




<style>
.familyfold-enter-active,
.familyfold-leave-active {
  transition: opacity 0.1s ease;
}

.familyfold-enter-from,
.familyfold-leave-to {
  opacity: 0;
}
</style>