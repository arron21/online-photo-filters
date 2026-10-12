<script setup lang="ts">
import FilterName from '../components/FilterName.vue'
import { ref, useTemplateRef } from 'vue'
import { toJpeg } from 'html-to-image'
import { saveAs } from 'file-saver'

const props = defineProps<{
  userImage: string
  filterName: string
}>()

const baseImgRef = useTemplateRef<HTMLImageElement>('baseImg')
const photoFrameRef = useTemplateRef<HTMLElement>('photoFrame')
const isDownloading = ref(false)

const tapDownloadImg = async () => {
  const el = photoFrameRef.value
  const baseImg = baseImgRef.value
  if (!el || !baseImg) return

  isDownloading.value = true
  try {
    const scaledWidth = (baseImg.naturalWidth || baseImg.clientWidth || 300) * 1.5
    const scaledHeight = (baseImg.naturalHeight || baseImg.clientHeight || 300) * 1.5

    const dataUrl = await toJpeg(el, {
      canvasWidth: scaledWidth,
      canvasHeight: scaledHeight,
      quality: 0.95
    })

    const filterClean = props.filterName.replace(/^filter-/, '')
    const fileName = `photo-${filterClean}-${Date.now()}.jpg`
    saveAs(dataUrl, fileName)
  } catch (error) {
    console.error('Download error:', error)
  } finally {
    isDownloading.value = false
  }
}
</script>

<template>
  <div class="raster-card raster-card-gallery">
    <div class="gallery-card-header">
      <FilterName :filterName="filterName" />
      <span class="raster-tag raster-mono">[ PRESET ]</span>
    </div>

    <div ref="photoFrame" class="gallery-frame">
      <img
        ref="baseImg"
        :class="filterName"
        v-if="userImage"
        :src="userImage"
        alt="Filtered output"
        class="gallery-img"
      />
    </div>

    <div class="gallery-card-footer">
      <button
        @click="tapDownloadImg"
        :disabled="isDownloading"
        class="raster-btn raster-btn-outline raster-btn-sm w-full"
      >
        <span class="raster-mono">{{ isDownloading ? '[ EXPORTING... ]' : '[ EXPORT JPG ]' }}</span>
      </button>
    </div>
  </div>
</template>

<style scoped>
.raster-card-gallery {
  padding: var(--raster-space-3);
  display: flex;
  flex-direction: column;
  gap: var(--raster-space-3);
  background-color: var(--raster-surface);
  border: 1px solid var(--raster-border);
  border-radius: var(--raster-radius);
  width: 240px;
}

.gallery-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.gallery-frame {
  position: relative;
  width: 100%;
  height: 200px;
  background-color: var(--raster-canvas);
  border: 1px solid var(--raster-border-subtle);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.gallery-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.gallery-card-footer {
  margin-top: auto;
}
</style>
