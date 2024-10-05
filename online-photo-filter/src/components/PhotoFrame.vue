<script setup lang="ts">
import { ref, useTemplateRef, onMounted } from 'vue'
import * as htmlToImage from 'html-to-image'
import { toJpeg } from 'html-to-image'
import { saveAs } from 'file-saver'


const displayName = ref()
const props = defineProps<{
  userImage: any,
  filterImage: any,
  filterPath: string,
  globalFilterMode: string | undefined,
  globalBlendMode: string,
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
      const now = new Date();
      const minutes = now.getMinutes();
      const seconds = now.getSeconds();

      var img = new Image()
      img.src = dataUrl
      if (window.saveAs) {
        window.saveAs(dataUrl, `photo-${minutes}-${seconds}.jpg`)
      } else {
        saveAs(dataUrl, `photo-${minutes}-${seconds}.jpg`)
      }
    })
    .catch(function (error: any) {
      console.error('oops, something went wrong!', error)
    })
}

</script>

<template>
  <div class="frame-container">
    <div ref="photoFrame" class="frame">
      <img
        :src="filterPath + filterImage + '.jpg'"
        :style="{ 'mix-blend-mode': globalBlendMode }"
        class="overlay"
        alt=""
      />
      <img :class="globalFilterMode" class="filtered-img" v-if="userImage" :src="userImage" />
      <img ref="baseImg" class="base" v-if="userImage" :src="userImage" />
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
