<script setup lang="ts">
import FilterName from '../components/FilterName.vue'
import { ref, useTemplateRef, onMounted } from 'vue'
import * as htmlToImage from 'html-to-image'
import { toJpeg } from 'html-to-image'
import { saveAs } from 'file-saver'


const displayName = ref()
const props = defineProps<{
  userImage: any,
  filterName: string
}>()

const baseImgRef = useTemplateRef('baseImg')
const photoFrameRef = useTemplateRef('photoFrame')

const tapDownloadImg = (event: any) => {
  // .closest is a cool function but we can just use useTemplateRef
  // const el = event.target.closest('.frame')

  const el = photoFrameRef.value;
  const baseImg = baseImgRef.value
  const scaledWidth = baseImg.clientWidth * 2
  const scaledHeight = baseImg.clientHeight * 2

  htmlToImage
    .toJpeg(el, {canvasWidth: scaledWidth, canvasHeight: scaledHeight})
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

</script>

<template>
  <div class="frame-container">
    <FilterName :filterName="filterName" />
    <div ref="photoFrame"  class="frame">
      <img ref="baseImg" :class="filterName" v-if="userImage" :src="userImage" />
    </div>

    <div class="frame-controls">
      <button @click="tapDownloadImg">download</button>
    </div>
  </div>
</template>

<style scoped>
.frame-container {
  position: relative;
}
.frame {
  cursor: pointer;
  position: relative;
}

.frame-controls {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 44px;
  z-index: 10;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
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
</style>
