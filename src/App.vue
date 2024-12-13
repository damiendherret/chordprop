<script setup>
import Chord from "./components/Chord.vue";
import ChordProposalFamily from "./components/ChordProposalFamily.vue";
import { reactive,ref } from 'vue';

import * as Tone from "tone";

//--------------------------------------------------------------------------------------------------------
// Constants
//
//--------------------------------------------------------------------------------------------------------
const sharpChromaticsNotes = ["C", "C#", "D", "D#", "E", "F", "F#", "G", "G#", "A", "A#", "B"];
const flatChromaticsNotes = ["C", "Db", "D", "Eb", "E", "F", "Gb", "G", "Ab", "A", "Bb", "B"];
const usesSharp = ["C", "G", "D", "A", "E", "B", "F#"];
const usesFlat = ["F", "Bb", "Eb", "Ab", "Db"];


//--------------------------------------------------------------------------------------------------------
// Scales
//
//--------------------------------------------------------------------------------------------------------
const scales = [];
const majorScale = {
  id: "MAJOR",
  name: "Major Scale",    
  chordsQuality: ["M","m","m","M","M","m","dim"],
  intervals: ["2","2","1","2","2","2","1"]
}; 
scales.push(majorScale);

const harmonicMinorScale = {
  id: "HMINOR",
  name: "Harmonic Minor Scale",    
  chordsQuality: ["m","dim","aug","m","M","M","dim"],
  intervals: ["2","1","2","2","1","2","2"]
}; 
scales.push(harmonicMinorScale);








//--------------------------------------------------------------------------------------------------------
// Utils Fonctions
//
//--------------------------------------------------------------------------------------------------------

function numeralsToRoman(degre,quality){
  var toReturn;
  switch (degre) {
    case 1:
      if (quality=="M") toReturn="I"
      else if (quality=="m") toReturn="i"
      else toReturn="I"+quality
      break;
    case 2:
      if (quality=="M") toReturn="II"
      else if (quality=="m") toReturn="ii"
      else toReturn="II"+quality
      break;
    case 3:
      if (quality=="M") toReturn="III"
      else if (quality=="m") toReturn="iii"
      else toReturn="III"+quality
      break;
    case 4:
      if (quality=="M") toReturn="IV"
      else if (quality=="m") toReturn="iv"
      else toReturn="IV"+quality
      break;
    case 5:
      if (quality=="M") toReturn="V"
      else if (quality=="m") toReturn="v"
      else toReturn="V"+quality
      break;
    case 6:
      if (quality=="M") toReturn="VI"
      else if (quality=="m") toReturn="vi"
      else toReturn="VI"+quality
      break;
    case 7:
      if (quality=="M") toReturn="VII"
      else if (quality=="m") toReturn="vii"
      else toReturn="VII"+quality
      break;
    default:
      toReturn = "-";
      break;
  }
  return toReturn;
}


const scaleColors = new Map();
var colorcount = 1;
function getColorForTonicScale(tonic,scale){
  if (scaleColors.has(tonic+scale.id)){
    return scaleColors.get(tonic+scale.id);
  } 
  else{
    scaleColors.set(tonic+scale.id,colorcount);
    colorcount++;
    return scaleColors.get(tonic+scale.id)
  }
}

//--------------------------------------------------------------------------------------------------------
// Feature Fonctions
//
//--------------------------------------------------------------------------------------------------------

function addChordToprogression(){
  var newChord = {
    val: "D",
    harmony: "bb"
  }
  console.log("chordProgression : "+ chordProgression.length);
  chordProgression.push(newChord);
  console.log("chordProgression : "+ chordProgression.length);
}



function addFamilyToPropositions(tonic,scale,degree){
  var family  = {
    familyName: tonic + " " + scale.name,
    scaleName: scale.name,
    scaleTonic: tonic,
    scaleMode: scale.chordsQuality[0],
    chords: []
  }
  
  var color = getColorForTonicScale(tonic,scale);

  var allnotes;
  if (usesSharp.includes(tonic)) allnotes=sharpChromaticsNotes
  else if (usesFlat.includes(tonic)) allnotes=flatChromaticsNotes
  var tonicIndex = allnotes.indexOf(tonic);

  var interval=0;
  for (let index = 0; index < 7; index++) { 
    if (index!=0) interval += parseInt(scale.intervals[index-1])
    var newNoteIndex = (tonicIndex+interval)%12
    //console.log("index : " + index + " / tonicIndex : " + tonicIndex + " / interval : " + interval);
    var chord = {
      val: allnotes[newNoteIndex],
      quality: scale.chordsQuality[index], //M, m, dim, aug
      harmony: numeralsToRoman(degree,scale.chordsQuality[degree-1]) + " > " + numeralsToRoman(index+1,scale.chordsQuality[index]),
      color: color
    }
    family.chords.push(chord);
  }
  chordProposalFamilies.push(family);
  
}


function getTonicFromScaleandDegree(chordRef,scale,degree){

  var interval=0;

  for (let index = 0 ; index < degree-1; index++) { 
    interval += parseInt(scale.intervals[index]);
  }
  console.log("Interval calculé : " + interval);

  //on cherche dans sharp, dans flat
  //si les deux sont le même > nickel
  //sinon on garde celui qui est présent dans usesSharp ou usesFlat
  var candidates = [];
  if (sharpChromaticsNotes.includes(chordRef.val)){
    var position = sharpChromaticsNotes.indexOf(chordRef.val);
    candidates.push(sharpChromaticsNotes.at(position-interval));
    console.log("Candidat pour sharp: " + sharpChromaticsNotes.at(position-interval));
  }
  if (flatChromaticsNotes.includes(chordRef.val)){
    var position = flatChromaticsNotes.indexOf(chordRef.val);
    candidates.push(flatChromaticsNotes.at(position-interval));
    console.log("Candidat pour flat: " + flatChromaticsNotes.at(position-interval));
  }

  var candidatToReturn;
  candidates.forEach(candidat => {
    if (usesSharp.includes(candidat)) candidatToReturn=candidat;
    if (usesFlat.includes(candidat)) candidatToReturn=candidat;
  });

  if(!candidatToReturn){
    if (sharpChromaticsNotes.includes(candidates [0])) candidatToReturn=flatChromaticsNotes[sharpChromaticsNotes.indexOf(candidates [0])];
    if (flatChromaticsNotes.includes(candidates [0])) candidatToReturn=sharpChromaticsNotes[flatChromaticsNotes.indexOf(candidates [0])];
    console.log("Pas de condidat on doit switch vers equivalent (" + candidates [0] + ") : " + candidatToReturn);
  }

  return candidatToReturn;

  

}

function proposeChords(chordRef){

  //re-init propositions
  chordProposalFamilies.length = 0;

  console.log("Nouvel Accord : " + chordRef.val + chordRef.quality);

  //scales management
  for (let index = 0; index < scales.length; index++) {
    const scale = scales[index];
    console.log("Traitement de : " + scale.name);
    
    //dans chaque gamme, on doit trouver quel degré on est
    let indexDegre=0;
    while (indexDegre>-1){
      indexDegre = scale.chordsQuality.indexOf(chordRef.quality,indexDegre);
      if (indexDegre!=-1){
        var degree = parseInt(indexDegre)+1;
        indexDegre=degree;
        console.log("Degré compatible : " + degree);
        var tonic = getTonicFromScaleandDegree(chordRef,scale,degree)
        console.log("Tonic retenue: " + tonic);

        addFamilyToPropositions(tonic,scale,degree);

        //var newFamily = createFamilyBasedOnChordAndDegree(chordRef,degree);
      }
    }
  }
}




function chordClickedDetected(chord, scaleName, tonic){
  console.log("I am parent and received : " + chord.val + chord.quality + " - " + tonic + " " + scaleName );

  // si c'est ele deuxième accord; on doit mmetre à jour la couleur et la onctin du premier accord
  if (chordProgression.length==1){
    if (scaleName) { // si c'est dans le cas d'un accord de gamme, sinon le premier accord reste indéfini
      console.log("Change first");
      var firstChord = chordProgression.pop();
      firstChord.color = chord.color;
      firstChord.harmony = chord.harmony.split('>')[0].trim();
      chordProgression.push(firstChord);

      chord.harmony = chord.harmony.split('>')[1].trim();
    }
    
  }

  // on ajoute l'accord à la progression
  chordProgression.push(chord);
  
  playChord(chord);

  // on lance le nouveau calcul
  proposeChords(chord);


}


function playSound(chord){
  //val: "C",
  //quality: "M", //M, m, dim, aug


  var root = chord.val + "4";

  var notes;
  (sharpChromaticsNotes.includes(chord.val)) ? notes = sharpChromaticsNotes : notes = flatChromaticsNotes;

  var intervalThird = 0;
  var intervalFifth = 0;
  var index = notes.indexOf(chord.val);

  switch (chord.quality) {
    case "M":
      intervalThird = 4;
      intervalFifth = 7;
      break;
    case "m":
      intervalThird = 3;
      intervalFifth = 7;
      break;
    case "dim":
      intervalThird = 3;
      intervalFifth = 6;
      break;
    case "aug":
      intervalThird = 4;
      intervalFifth = 8;
      break;
    default:
      intervalThird = 4;
      intervalFifth = 7;
      break;
  }




  var third = notes.at((index+intervalThird)%12) + "4";
  var fifth = notes.at((index+intervalFifth)%12) + "4";
  console.log("index : " + index );
  console.log("intervalThird : " + intervalThird );
  console.log("intervalFifth : " + intervalFifth );
  console.log("Paying : " + root + " / " + third + " / " + fifth);

  const synth = new Tone.PolySynth(Tone.Synth).toDestination();
  const now = Tone.now();
  synth.triggerAttack(root, now);
  synth.triggerAttack(third, now);
  synth.triggerAttack(fifth, now);
  synth.triggerRelease([root, third, fifth], now + 0.5);
}

function playChord(chord){
  //const synth = new Tone.PolySynth(Tone.Synth).toDestination();
  //const now = Tone.now();
  //synth.triggerAttack("C4", now);
  //synth.triggerAttack("Eb4", now);
  //synth.triggerAttack("G4", now);
  //synth.triggerRelease(["C4", "Eb4", "G4"], now + 0.5);
  if (audioChecked.value==true) playSound(chord)

}


function stopStartAudioEngine(){
  console.log("audioChecked : " + audioChecked.value);
  if (audioChecked.value==false){ //hmm vue v-model must change value afterward...
    console.log("Starting audio ... ");
    Tone.start();
	  console.log("... audio is ready");
  }
  else{
    console.log("Stoping audio ");	  
  }

}

//--------------------------------------------------------------------------------------------------------
// Main
//
//--------------------------------------------------------------------------------------------------------

// Chord Progression init
const chordProgression = reactive([]); 
var firstChord = {
  val: "Bb",
  quality: "m", //M, m, dim, aug
  harmony: "-",
  color: 0
}
chordProgression.push(firstChord);

//Chord proposal
const chordProposalFamilies = reactive([]); 
proposeChords(firstChord);

//Audio
const audioChecked = ref(false);





</script>

<template>

<button @click="addChordToprogression">Add chord !! </button>
<button @click="proposeChords">ProposeChord </button>
<button @click="playSound">PlaySound </button>

<div class="app-title fa">Chord Proposer</div>
<div class="audio"> 
  <div class="audioText">Audio : &nbsp </div>
  <div class="audioButton">
    <label class="switch">
      <input type="checkbox" v-model="audioChecked" @click="stopStartAudioEngine">
      <span class="slider round"></span>
    </label>
  </div>
</div>


<div class="chord-area">

  <div class="chord-progression">
    <Chord v-for="chord in chordProgression" v-bind:chord="chord" progression="true"/>
  </div>

  <div class="nextchord-proposal">

    <ChordProposalFamily v-for="chordFamily in chordProposalFamilies" v-bind:chordFamily="chordFamily" v-on:chordClicked="chordClickedDetected"/>



  </div>

</div>

</template>