<script setup lang="ts">
import { ref, onMounted } from 'vue'
import FilterName from '../components/FilterName.vue'
import PhotoFrame from '../components/PhotoFrame.vue'
import PhotoBasicFilter from '../components/PhotoBasicFilter.vue'

const blendMode = ref('soft-light')
let selectedImg = ref('/img/squirell.jpg')

const globalBlendMode = ref('soft-light')
const globalFilterMode = ref('filter-core')
const filterMultiOptions = Array.from({ length: 15 }, (_, i) => i + 1)
const lightLeakColors = ['Cyan', 'Magenta', 'Orange', 'Red', 'Yellow']

const selectedFilter = ref('/filters/1.jpg')
const filterNames = [
  'filter-core',
  'filter-1977',
  'filter-aden',
  'filter-amaro',
  'filter-ashby',
  'filter-brannan',
  'filter-brooklyn',
  'filter-charmes',
  'filter-clarendon',
  'filter-crema',
  'filter-dogpatch',
  'filter-earlybird',
  'filter-gingham',
  'filter-ginza',
  'filter-hefe',
  'filter-helena',
  'filter-hudson',
  'filter-inkwell',
  'filter-juno',
  'filter-kelvin',
  'filter-lark',
  'filter-lofi',
  'filter-ludwig',
  'filter-maven',
  'filter-mayfair',
  'filter-moon',
  'filter-nashville',
  'filter-perpetua',
  'filter-poprocket',
  'filter-reyes',
  'filter-rise',
  'filter-sierra',
  'filter-skyline',
  'filter-slumber',
  'filter-stinson',
  'filter-sutro',
  'filter-toaster',
  'filter-valencia',
  'filter-vesper',
  'filter-walden',
  'filter-willow',
  'filter-xpro-ii'
]

const blendModes = [
  'normal',
  'multiply',
  'screen',
  'overlay',
  'darken',
  'lighten',
  'color-dodge',
  'color-burn',
  'hard-light',
  'soft-light',
  'difference',
  'exclusion',
  'hue',
  'saturation',
  'color',
  'luminosity'
]

onMounted(() => {
  window.addEventListener('keydown', onPageDown)
})

const onSetSelectedImg = (img: string) => {
  const file = event.target.files[0];
  if (file) {
    selectedImg.value = URL.createObjectURL(file);
  }
}

const changeFilterLeft = () => {
  if (filterNames.indexOf(globalFilterMode.value) === -1) {
    globalFilterMode.value = filterNames[filterNames.length - 1]
  } else {
    globalFilterMode.value = filterNames[filterNames.indexOf(globalFilterMode.value) - 1]
  }
}

const changeFilterRight = () => {
  if (filterNames.indexOf(globalFilterMode.value) === filterNames.length - 1) {
    globalFilterMode.value = filterNames.length
  }
  globalFilterMode.value = filterNames[filterNames.indexOf(globalFilterMode.value) + 1]
}

const changeBlendModeLeft = () => {
  if (blendModes.indexOf(globalBlendMode.value) === -1) {
    globalBlendMode.value = blendModes[blendModes.length - 1]
  } else {
    globalBlendMode.value = blendModes[blendModes.indexOf(globalBlendMode.value) - 1]
  }
}

const changeBlendModeRight = () => {
  if (blendModes.indexOf(globalBlendMode.value) === blendModes.length - 1) {
    globalBlendMode.value = blendModes.length
  }
  globalBlendMode.value = blendModes[blendModes.indexOf(globalBlendMode.value) + 1]
}

const onPageDown = (e: any) => {
  if (e.keyCode === 90) {
    changeFilterLeft()
  }
  if (e.keyCode === 88) {
    changeFilterRight()
  }
  if (e.keyCode === 37) {
    changeBlendModeLeft()
  }
  if (e.keyCode === 39) {
    changeBlendModeRight()
  }
}
</script>

<template>

  <div class="filters-sections">
    <section>
      <h1>Upload your photo</h1>
      <input accept="image/*" @change="onSetSelectedImg" type="file" />
    </section>

    <section>
      <h2>Basic Filters</h2>
      <div class="filters-container">
        <div v-for="n in filterNames">
          <PhotoBasicFilter :filterName="n" :userImage="selectedImg" />
        </div>
      </div>
    </section>

    <section>
      <h2>Multi Directional Light Leak Filters</h2>
      <div class="filters-container">
        <div v-for="n in filterMultiOptions">
          <PhotoFrame :filterImage="n" :filterPath="'/lightleak1/Multi-'" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 1 Filters</h2>
      <div class="filters-container">
        
        <div v-for="n in lightLeakColors">
          <PhotoFrame :filterImage="n" :filterPath="'/Light Leak 1/Light Leak 1 - '" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />

        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 2 Filters</h2>
      <div class="filters-container">
        <div v-for="n in lightLeakColors">
          <PhotoFrame :filterImage="n" :filterPath="'/Light Leak 2/Light Leak 2 - '" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 3 Filters</h2>
      <div class="filters-container">
        <div v-for="n in lightLeakColors">
          <PhotoFrame :filterImage="n" :filterPath="'/Light Leak 3/Light Leak 3 - '" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />

        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 4 Filters</h2>
      <div class="filters-container">
        <div v-for="n in lightLeakColors">
          <PhotoFrame :filterImage="n" :filterPath="'/Light Leak 4/Light Leak 4 - '" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 5 Filters</h2>
      <div class="filters-container">
        <div v-for="n in lightLeakColors">
          <PhotoFrame :filterImage="n" :filterPath="'/Light Leak 5/Light Leak 5 - '" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 6 Filters</h2>
      <div class="filters-container">
        <div v-for="n in lightLeakColors">
          <PhotoFrame :filterImage="n" :filterPath="'/Light Leak 6/Light Leak 6 - '" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 7 Filters</h2>
      <div class="filters-container">
        <div v-for="n in lightLeakColors">
          <PhotoFrame :filterImage="n" :filterPath="'/Light Leak 7/Light Leak 7 - '" :userImage="selectedImg" :globalBlendMode="globalBlendMode" :globalFilterMode="globalFilterMode" />
        </div>
      </div>
    </section>
  </div>
  <div class="global-controls">
    <div>

      <div>
        <div>
          z - x
        </div>
        <select v-model="globalFilterMode" id="filterMode">
          <option disabled value="">Please select one</option>
          <option v-for="n in filterNames">{{ n }}</option>
        </select>
      </div>
      
      <div>
        
        <div>
          left arrow - right arrow
        </div>
        <select v-model="globalBlendMode" id="blendModes">
          <option disabled value="">Please select one</option>
          <option v-for="n in blendModes">{{ n }}</option>
        </select>
      </div>
    </div>
    </div>
    
  </template>

<style>
section {
  padding: 1rem;
  margin: 1rem;
  background-color: #191919;
  border-radius: 1rem;
  /* box-shadow: 2px 4px 0px #222; */
  &:after {
  }
  max-width: calc(100vw - 3em);
  @media screen and (max-width: 1024px) { 
    width: 100%;
    margin: 1rem auto;
  }
}

.filters-sections {

  /* margin-bottom: 360px; */
}

.global-controls {
  position: sticky;
  bottom: 44px;
  max-width: 100%;
  z-index: 100;
  text-align: center;
    padding: .5rem;
  display: flex;
  justify-content: space-evenly;
  > div {
    text-align: center;
    padding: .5rem;
  background-color: #191919;
  display: flex;
  border-radius: 50px;
  justify-content: space-evenly;

  gap: 1rem;
  padding: .5rem 1.5rem;
  }
  @media screen and (max-width: 1024px) { 
  }

  select {
    background: #111111;
    color: white;
    border: none;
    padding: 4px;
    margin: 4px;
    border-radius: 1rem;
    text-align: center;
    appearance: none;
    position: relative;

    &:after {
      content: '\21C5';
      position: absolute;
      right: 0;
      top:0;
      display: block;
    }
  }

}

</style>
