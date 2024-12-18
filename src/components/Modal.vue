<script setup>
import { reactive,ref } from 'vue';
const props = defineProps({
  show: Boolean
})


const note = ref("A");
const noteBis = ref("");
const quality = ref("");

const buttonClass = "chordConfig";

const picked = ref('One')

/*
:class="{ selected: isActive, 'text-danger': hasError }"

chordConfig 
active: isActive
*/
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



            <div>Picked: {{ picked }}</div>
            <input type="radio" id="one" value="One" v-model="picked" />
            <label for="one">One</label>

            <input type="radio" id="two" value="Two" v-model="picked" />
            <label for="two">Two</label>
            <br/>


            

            <div class="checkbox-button">
              <label>
                <input type="checkbox" value="1"><span>RED</span>
              </label>
            </div>







            <button class="chordConfig">A</button>
            <button class="chordConfig">B</button>
            <button class="chordConfig">C</button>
            <button class="chordConfig">D</button>
            <button class="chordConfig">E</button>
            <button class="chordConfig">F</button>
            <button class="chordConfig">G</button><br/>
            <button class="chordConfig">&#x266E;</button>
            <button class="chordConfig">#</button>
            <button class="chordConfig">b</button><br/>
            <button class="chordConfig">M</button>
            <button class="chordConfig">m</button>
            <button class="chordConfig">dim</button>
            <button class="chordConfig">aug</button><br/>            
          </slot>
        </div>

        <div class="modal-footer">
          <slot name="footer">
            default footer
            <button
              class="modal-default-button"
              @click="$emit('close')"
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
  width: 300px;
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
  margin: 20px 0;
}

.modal-default-button {
  float: right;
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









.checkbox-button {
    margin:4px;
    background-color:#333;
    border-radius:4px;
    border:1px solid #000;
    overflow:auto;
    float:left;
    color: #fff;
    
}

.checkbox-button label {
    float:left;
    width:100px;
    cursor: pointer;
}

.checkbox-button label span {
    text-align:center;
    padding:5px 10px;
    display:block;
    border-radius:4px;
}

.checkbox-button label input {
    position:absolute;
    top:-20px;
}

.checkbox-button input:hover + span {
    background-color:#efE0E0;
    color: #333;
}

.checkbox-button input:checked + span {
    background-color:#911;
    color:#fff;
}

.checkbox-button input:checked:hover + span {
    background-color:#c11;
    color:#fff;
}





</style>