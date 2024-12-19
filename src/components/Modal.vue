<script setup>
import { reactive,ref } from 'vue';
const props = defineProps({
  show: Boolean
})


const note = ref("A");
const noteBis = ref("");
const quality = ref("");


const pickedChord = ref('C');
const pickedAccidental = ref('becar');
const pickedQuality  = ref('M');

</script>

<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-container">
        <div class="modal-header">
          <slot name="header">default header</slot>
        </div>

        <div class="modal-body">
          <slot name="body">
            <ul class="chord-selector-chord">
              <li>
                <input type="radio" id="A" value="A" name="root" v-model="pickedChord" />
                <label for="A">A</label>
              </li>
              <li>
                <input type="radio" id="B" value="B" name="root" v-model="pickedChord" />
                <label for="B">B</label>
              </li>
              <li>
                <input type="radio" id="C" value="C" name="root" v-model="pickedChord" />
                <label for="C">C</label>
              </li>
              <li>
                <input type="radio" id="D" value="D" name="root" v-model="pickedChord" />
                <label for="D">D</label>
              </li>
              <li>
                <input type="radio" id="E" value="E" name="root" v-model="pickedChord" />
                <label for="E">E</label>
              </li>
              <li>
                <input type="radio" id="F" value="F" name="root" v-model="pickedChord" />
                <label for="F">F</label>
              </li>
              <li>
                <input type="radio" id="G" value="G" name="root" v-model="pickedChord" />
                <label for="G">G</label>
              </li>
            </ul>
          </slot>
        </div>
        <div class="modal-body">
          <slot name="body">
            <ul class="chord-selector-chord">
              <li>
                <input type="radio" id="becar" value="becar" name="accidentals" v-model="pickedAccidental" />
                <label for="becar">&#9838</label>
              </li>
              <li>
                <input type="radio" id="diese" value="diese" name="accidentals" v-model="pickedAccidental" />
                <label for="diese">#</label>
              </li>
              <li>
                <input type="radio" id="bemol" value="bemol" name="accidentals" v-model="pickedAccidental" />
                <label for="bemol">b</label>
              </li>
            </ul>
          </slot>
        </div>
        <div class="modal-body">
          <slot name="body">
            <ul class="chord-selector-chord">
              <li>
                <input type="radio" id="M" value="M" name="quality" v-model="pickedQuality" />
                <label for="M">M</label>
              </li>
              <li>
                <input type="radio" id="m" value="m" name="quality" v-model="pickedQuality" />
                <label for="m">m</label>
              </li>
              <li>
                <input type="radio" id="dim" value="dim" name="quality" v-model="pickedQuality" />
                <label for="dim">dim</label>
              </li>
              <li>
                <input type="radio" id="aug" value="aug" name="quality" v-model="pickedQuality" />
                <label for="aug">aug</label>
              </li>
            </ul>

          </slot>
        </div>



        <div class="modal-footer">
          <slot name="footer">
            <button
              class="modal-default-button"
              @click="$emit('close', pickedChord, pickedAccidental, pickedQuality)"
            >OK</button>
          </slot>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  transition: opacity 0.3s ease;
}

.modal-container {
  display: flex;
  flex-direction: column;
  width: 500px;
  margin: auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 5px 0;
}

.modal-default-button {
  float: right;
  font-size: 25px;
  width: 70px;
  border-radius: 10px;
  background: linear-gradient(to bottom left,#f3c278, #ff7300);
}

.modal-default-button:hover {
  background: rgb(164, 164, 231);
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}






.chord-selector-chord {
  list-style-type: none;
  margin: 0 0 0 0;
  padding: 0;
  
}

.chord-selector-chord li {
  float: left;
  margin: 0 5px 0 0;
  width: 40px;
  height: 30px;
  position: relative;
}


.chord-selector-chord label,
.chord-selector-chord input {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  text-align: center;
  cursor: pointer;
}

.chord-selector-chord label {
  padding: 3px;
  border: 2px solid #CCC;
  border-radius: 10px;
  z-index: 90;
}


.chord-selector-chord input[type="radio"] {
  opacity: 0.01;
  z-index: 100;
}

.chord-selector-chord input[type="radio"]:checked+label,
.Checked+label {
  background: linear-gradient(to bottom left,#f3c278, #ff7300);
}

.chord-selector-chord input[type="radio"]:hover+label,
.Checked+label {
  background: rgb(164, 164, 231);
}




</style>