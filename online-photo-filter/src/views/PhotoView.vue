<script setup lang="ts">
import { ref } from 'vue'
import * as htmlToImage from 'html-to-image'
import { toJpeg } from 'html-to-image'
import { saveAs } from 'file-saver'

const blendMode = ref('soft-light')
let selectedImg = ref('')
const filterOptions = Array.from({ length: 20 }, (_, i) => i + 1)
const selectedFilter = ref('/filters/1.jpg')

const saveImage = () => {
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
      selectedImg.value = ref(e.target.result)
    }
    reader.readAsDataURL(file)
  }
}
</script>

<template>
  <div class="about">
    <h1>Upload your photo</h1>
    <input accept="image/*" @change="onSetSelectedImg" type="file" />
    <div>
      <select v-model="blendMode">
        <option disabled value="">Please select one</option>
        <option>normal</option>
        <option>multiply</option>
        <option>screen</option>
        <option>overlay</option>
        <option>darken</option>
        <option>lighten</option>
        <option>color-dodge</option>
        <option>color-burn</option>
        <option>hard-light</option>
        <option>soft-light</option>
        <option>difference</option>
        <option>exclusion</option>
        <option>hue</option>
        <option>saturation</option>
        <option>color</option>
        <option>luminosity</option>
      </select>
    </div>
    <select v-model="selectedFilter">
      <option disabled value="">Please select one</option>
      <option v-for="n in filterOptions">/filters/{{ n }}.jpg</option>
    </select>

    <div id="frame" class="frame">
      <img :src="selectedFilter" :style="{ 'mix-blend-mode': blendMode }" class="overlay" alt="" />
      <img v-if="selectedImg.value" :src="selectedImg.value" />
    </div>
    <button @click="saveImage">Download Image</button>
  </div>
</template>

<style scoped>
.frame {
  width: 400px;
  aspect-ratio: 1 / 1;
  overflow: hidden;

  position: relative;
  img {
    inset: 0;
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .overlay {
    opacity: 0.9;
    z-index: 2;
    mix-blend-mode: screen;
  }
}
</style>
