<script setup>

const props = defineProps({
  chord: Object,
  progression: String
});

const emit = defineEmits(['chordClicked']);


const mainClass="card-header";
const colorClass="color";


</script>


<template>
    <div class="card-wrap" @click="$emit('chordClicked', chord)">
      <div :class="[mainClass, colorClass+chord.color]">
      <!--div class="card-header color{{ chord.color || 0 }}"-->
        <i>{{ chord.val + ((chord.quality=="M") ? "" : chord.quality )|| '-' }}</i>
      </div>
      <div class="card-content">
        <p class="card-text">{{ chord.harmony || '-' }}</p>
      </div>
    </div>

    <div v-if="progression" class="next">></div>

</template>