<script setup lang="ts">
import { ref, useTemplateRef } from 'vue'
import { toJpeg } from 'html-to-image'
import { saveAs } from 'file-saver'

const props = defineProps<{
  userImage: string
  filterImage: string | number
  filterPath: string
  globalFilterMode?: string
  globalBlendMode: string
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

    const fileName = `lightleak-${props.filterImage}-${Date.now()}.jpg`
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
      <span class="raster-label raster-mono">LEAK // {{ filterImage }}</span>
      <span class="raster-tag raster-mono">[ FX ]</span>
    </div>

    <div ref="photoFrame" class="gallery-frame">
      <img
        :src="filterPath + filterImage + '.jpg'"
        :style="{ mixBlendMode: globalBlendMode as any }"
        class="gallery-overlay"
        alt="Overlay effect"
      />
      <img
        ref="baseImg"
        :class="globalFilterMode"
        v-if="userImage"
        :src="userImage"
        alt="Base photo"
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

.gallery-overlay {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.9;
  z-index: 2;
  pointer-events: none;
}

.gallery-card-footer {
  margin-top: auto;
}
</style>
