<script setup>

const props = defineProps({
  chord: Object,
  progression: Boolean,
  last: Boolean,
  first: Boolean
});

const emit = defineEmits(['chordClicked','removeFromProgression']);


const mainClass="card-header";
const colorClass="color";


</script>


<template>
    <div class="card-wrap" @click="$emit('chordClicked', chord, null, null, progression, first)">
      <div :class="[mainClass, colorClass+chord.color]">
      <!--div class="card-header color{{ chord.color || 0 }}"-->
        <i>{{ chord.val + ((chord.quality=="M") ? "" : chord.quality )|| '-' }}</i>
      </div>
      
      
      
      <div class="card-content">
        <p class="card-text">
          {{ chord.harmony || '-' }} <i v-if="last" 
                                        class="fa-regular fa-circle-xmark closeX"
                                        @click="$emit('removeFromProgression', chord)"
                                        ></i></p>
      </div>
    </div>

    <div v-if="progression" class="next"><i class="fa-solid fa-arrow-right playButton"></i></div>

</template>