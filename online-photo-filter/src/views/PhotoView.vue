<script setup lang="ts">
import { ref, onMounted } from 'vue'

import * as htmlToImage from 'html-to-image'
import { toJpeg } from 'html-to-image'
import { saveAs } from 'file-saver'

const blendMode = ref('soft-light')
let selectedImg = ref('/img/andrew-cats.jpg')

const globalBlendMode = ref('soft-light')

const defaultImg = ref('/img/andrew-cats.jpg')

const filterOptions = Array.from({ length: 28 }, (_, i) => i + 1)
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

const tapDownloadImg = (event: any) => {
  console.log(event)

  const el = event.target.parentElement

  htmlToImage
    .toJpeg(el)
    .then(function (dataUrl: any) {
      var img = new Image()
      img.src = dataUrl
      if (window.saveAs) {
        window.saveAs(dataUrl, 'my-node.jpg')
      } else {
        saveAs(dataUrl, 'my-node.jpg')
      }
    })
    .catch(function (error: any) {
      console.error('oops, something went wrong!', error)
    })
}

const saveImage = (img: any) => {
  console.log(img)
  const el = document.getElementById('frame')
  htmlToImage
    .toJpeg(el)
    .then(function (dataUrl: any) {
      var img = new Image()
      img.src = dataUrl
      if (window.saveAs) {
        window.saveAs(dataUrl, 'my-node.jpg')
      } else {
        saveAs(dataUrl, 'my-node.jpg')
      }
    })
    .catch(function (error: any) {
      console.error('oops, something went wrong!', error)
    })
}

const onSetSelectedImg = (event: any) => {
  const file = event.target.files[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (e: any) => {
      console.log(e)
      console.log(e.target.result)

      // TODO: figure out how to make sure change detection works
      // selectedImg = ref(e.target.result)
      selectedImg.value = e.target.result
    }
    reader.readAsDataURL(file)
  }
}

onMounted(() => {
  console.log(`the component is now mounted.`)
  console.log(defaultImg.value)
  // selectedImg.value = defaultImg.value
  window.addEventListener('keydown', onPageDown)
})

const setDefaultImg = () => {
  console.log(defaultImg)
  selectedImg.value = defaultImg.value
}

const changeFilterLeft = () => {
  if (blendModes.indexOf(globalBlendMode.value) === -1) {
    globalBlendMode.value = blendModes[blendModes.length - 1]
  } else {
    globalBlendMode.value = blendModes[blendModes.indexOf(globalBlendMode.value) - 1]
  }
}

const changeFilterRight = () => {
  console.log(blendModes.indexOf(globalBlendMode.value))
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
    changeFilterLeft()
  }
  if (e.keyCode === 39) {
    changeFilterRight()
  }
}
</script>

<template>
  <div class="global-controls">
    <select v-model="globalBlendMode" id="blendModes">
      <option disabled value="">Please select one</option>
      <option v-for="n in blendModes">{{ n }}</option>
    </select>
  </div>

  <div>
    <section>
      <h1>Upload your photo</h1>
      <input accept="image/*" @change="onSetSelectedImg" type="file" />
    </section>

    <section>
      <h2>Basic Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in filterNames">
          <img @click="tapDownloadImg" :class="n" v-if="selectedImg" :src="selectedImg" />
        </div>
      </div>
    </section>

    <section>
      <h2>Multi Directional Light Leak Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in filterMultiOptions">
          <div class="frame">
            <img
              :src="'/lightleak1/Multi-' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>

          <!-- <img :class="n" v-if="selectedImg" :src="selectedImg" /> -->
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 1 Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in lightLeakColors">
          <div class="frame">
            <img
              :src="'/Light Leak 1/Light Leak 1 - ' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 2 Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in lightLeakColors">
          <div class="frame">
            <img
              @click="tapDownloadImg"
              :src="'/Light Leak 2/Light Leak 2 - ' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 3 Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in lightLeakColors">
          <div class="frame">
            <img
              :src="'/Light Leak 3/Light Leak 3 - ' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 4 Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in lightLeakColors">
          <div class="frame">
            <img
              :src="'/Light Leak 4/Light Leak 4 - ' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 5 Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in lightLeakColors">
          <div class="frame">
            <img
              :src="'/Light Leak 5/Light Leak 5 - ' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 6 Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in lightLeakColors">
          <div class="frame">
            <img
              :src="'/Light Leak 6/Light Leak 6 - ' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>
        </div>
      </div>
    </section>

    <section>
      <h2>Light Leak 7 Filters</h2>
      <div class="instagram-filters">
        <div v-for="n in lightLeakColors">
          <div class="frame">
            <img
              :src="'/Light Leak 7/Light Leak 7 - ' + n + '.jpg'"
              :style="{ 'mix-blend-mode': globalBlendMode }"
              class="overlay"
              alt=""
            />
            <img class="filtered-img" v-if="selectedImg" :src="selectedImg" />
            <img class="base" v-if="selectedImg" :src="selectedImg" />
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
section {
  padding: 2em;
  margin: 2em;
  background-color: #333;
  border-radius: 2em;
  box-shadow: 2px 4px 0px #222;
  &:after {
  }
}
.frame {
  position: relative;
  display: inline-flex;
  .base,
  .filtered-img {
    max-width: 200px;
  }
  .filtered-img {
    inset: 0;
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
  }

  .overlay {
    position: absolute;
    opacity: 0.9;
    z-index: 2;
    height: 100%;
    width: 100%;

    mix-blend-mode: screen;
  }
}

.global-controls {
  position: fixed;
  bottom: 0;
  left: 0;
  width: calc(100% - 200px);
  height: 44px;
  z-index: 10;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.frame {
  cursor: pointer;
}
</style>
