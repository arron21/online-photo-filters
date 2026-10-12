<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted, useTemplateRef } from 'vue'
import { toJpeg } from 'html-to-image'
import { saveAs } from 'file-saver'
import PhotoFrame from '../components/PhotoFrame.vue'
import PhotoBasicFilter from '../components/PhotoBasicFilter.vue'

// Interface for Preset definition
interface Preset {
  id: string
  name: string
  subtitle: string
  filterClass: string
  blendMode?: string
  leakPath?: string
  category: 'featured' | 'classic' | 'leak'
  index: string
}

// Active Studio View Mode
const viewMode = ref<'studio' | 'gallery'>('studio')

// Current image state (default sample image provided)
const selectedImg = ref<string>('/img/arron-blue-zion.jpg')
const selectedImgDimensions = ref<string>('ORIGINAL')
const userFileName = ref<string>('arron-blue-zion.jpg')
const isDragOver = ref(false)
const fileInputRef = useTemplateRef<HTMLInputElement>('fileInput')

// Canvas references
const studioCanvasRef = useTemplateRef<HTMLElement>('studioCanvas')
const baseCanvasImgRef = useTemplateRef<HTMLImageElement>('baseCanvasImg')
const isExporting = ref(false)

// Active filter & effects state
const activePresetId = ref<string>('clean-base')
const globalFilterMode = ref<string>('filter-core')
const globalBlendMode = ref<string>('screen')
const activeLeakPath = ref<string>('')
const presetSearchQuery = ref<string>('')
const activeCategoryTab = ref<'featured' | 'classic' | 'leaks' | 'all'>('featured')

// Sample images
const sampleImages = [
  { name: 'Zion', path: '/img/arron-blue-zion.jpg' },
  { name: 'Lemur', path: '/img/leemur.jpeg' },
  { name: 'Cats', path: '/img/andrew-cats.jpg' },
  { name: 'Squirrel', path: '/img/squirell.jpg' }
]

// Blend Modes
const blendModes = [
  'screen',
  'overlay',
  'multiply',
  'normal',
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

// All 42 Classic Instagram / Film CSS filters
const rawFilterNames = [
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

// Light leak variations for gallery
const filterMultiOptions = Array.from({ length: 15 }, (_, i) => i + 1)
const lightLeakColors = ['Cyan', 'Magenta', 'Orange', 'Red', 'Yellow']

// 7 Signature Featured Presets directly addressing the user's mockup
const featuredPresets: Preset[] = [
  {
    id: 'clean-base',
    name: 'CLEAN BASE',
    subtitle: 'START FROM THE UNTOUCHED ORIGINAL.',
    filterClass: 'filter-core',
    category: 'featured',
    index: '01'
  },
  {
    id: 'crisp-pop',
    name: 'CRISP POP',
    subtitle: 'PUNCHY CONTRAST WITH VIBRANT COLORS.',
    filterClass: 'filter-clarendon',
    category: 'featured',
    index: '02'
  },
  {
    id: 'sunset-dream',
    name: 'SUNSET DREAM',
    subtitle: 'SOFT WARMTH WITH A DREAMY SHIFT.',
    filterClass: 'filter-valencia',
    leakPath: '/Light Leak 1/Light Leak 1 - Orange.jpg',
    blendMode: 'screen',
    category: 'featured',
    index: '03'
  },
  {
    id: 'aurora-veil',
    name: 'AURORA VEIL',
    subtitle: 'COOL CYAN HAZE WITH LIFTED BLACKS.',
    filterClass: 'filter-hudson',
    leakPath: '/Light Leak 2/Light Leak 2 - Cyan.jpg',
    blendMode: 'screen',
    category: 'featured',
    index: '04'
  },
  {
    id: 'portra-soft',
    name: 'PORTRA SOFT',
    subtitle: 'CREAMY HIGHLIGHTS WITH PASTEL WARMTH.',
    filterClass: 'filter-gingham',
    category: 'featured',
    index: '05'
  },
  {
    id: 'teal-crush',
    name: 'TEAL CRUSH',
    subtitle: 'AQUA SHADOWS AND CORAL HIGHLIGHTS.',
    filterClass: 'filter-brooklyn',
    category: 'featured',
    index: '06'
  },
  {
    id: 'midnight-grain',
    name: 'MIDNIGHT GRAIN',
    subtitle: 'FILM NOIR GRADIENT WITH ORGANIC NOISE.',
    filterClass: 'filter-inkwell',
    category: 'featured',
    index: '07'
  }
]

// Generate classic presets list from rawFilterNames
const classicPresets: Preset[] = rawFilterNames
  .filter((f) => f !== 'filter-core')
  .map((name, i) => {
    const clean = name.replace(/^filter-/, '').toUpperCase()
    return {
      id: name,
      name: clean,
      subtitle: `ANALOG TONE CURVE // EMULATION [${clean}]`,
      filterClass: name,
      category: 'classic' as const,
      index: String(i + 8).padStart(2, '0')
    }
  })

// Light leak presets
const leakPresets: Preset[] = [
  {
    id: 'leak-multi-01',
    name: 'MULTI LEAK 01',
    subtitle: 'ANALOG MULTI-DIRECTIONAL PRISM LEAK.',
    filterClass: 'filter-core',
    leakPath: '/lightleak1/Multi-1.jpg',
    blendMode: 'screen',
    category: 'leak',
    index: 'L1'
  },
  {
    id: 'leak-multi-05',
    name: 'MULTI LEAK 05',
    subtitle: 'VIVID CHROMATIC LIGHT EXPOSURE.',
    filterClass: 'filter-core',
    leakPath: '/lightleak1/Multi-5.jpg',
    blendMode: 'screen',
    category: 'leak',
    index: 'L2'
  },
  {
    id: 'leak-warm-flare',
    name: 'WARM FLARE',
    subtitle: 'VINTAGE 35MM CORNER SUNBURST.',
    filterClass: 'filter-rise',
    leakPath: '/Light Leak 1/Light Leak 1 - Orange.jpg',
    blendMode: 'screen',
    category: 'leak',
    index: 'L3'
  },
  {
    id: 'leak-cyan-haze',
    name: 'CYAN HAZE',
    subtitle: 'SUBTLE ANAMORPHIC HORIZONTAL STREAK.',
    filterClass: 'filter-aden',
    leakPath: '/Light Leak 3/Light Leak 3 - Cyan.jpg',
    blendMode: 'screen',
    category: 'leak',
    index: 'L4'
  },
  {
    id: 'leak-magenta-glow',
    name: 'MAGENTA GLOW',
    subtitle: 'DREAMY EXPERIMENTAL COLOR DUST.',
    filterClass: 'filter-valencia',
    leakPath: '/Light Leak 4/Light Leak 4 - Magenta.jpg',
    blendMode: 'screen',
    category: 'leak',
    index: 'L5'
  }
]

// All combined presets
const allPresets: Preset[] = [...featuredPresets, ...classicPresets, ...leakPresets]

// Filtered list based on category tab & search query
const visiblePresets = computed(() => {
  let list = allPresets
  if (activeCategoryTab.value === 'featured') {
    list = featuredPresets
  } else if (activeCategoryTab.value === 'classic') {
    list = classicPresets
  } else if (activeCategoryTab.value === 'leaks') {
    list = leakPresets
  }

  if (presetSearchQuery.value.trim()) {
    const q = presetSearchQuery.value.toLowerCase()
    list = list.filter((p) => p.name.toLowerCase().includes(q) || p.subtitle.toLowerCase().includes(q))
  }
  return list
})

// Active preset object
const activePreset = computed(() => {
  return allPresets.find((p) => p.id === activePresetId.value) || featuredPresets[0]
})

// Apply preset
const applyPreset = (preset: Preset) => {
  activePresetId.value = preset.id
  globalFilterMode.value = preset.filterClass
  if (preset.leakPath) {
    activeLeakPath.value = preset.leakPath
    if (preset.blendMode) {
      globalBlendMode.value = preset.blendMode
    }
  } else {
    activeLeakPath.value = ''
  }
}

// Reset all filters to clean base
const resetAll = () => {
  applyPreset(featuredPresets[0])
  globalBlendMode.value = 'screen'
  activeLeakPath.value = ''
}

// File Input Handling
const triggerFileInput = () => {
  if (fileInputRef.value) {
    fileInputRef.value.click()
  }
}

const onFileSelected = (e: Event) => {
  const target = e.target as HTMLInputElement
  if (target.files && target.files[0]) {
    const file = target.files[0]
    selectedImg.value = URL.createObjectURL(file)
    userFileName.value = file.name
  }
}

const onDragOver = (e: DragEvent) => {
  e.preventDefault()
  isDragOver.value = true
}

const onDragLeave = () => {
  isDragOver.value = false
}

const onDrop = (e: DragEvent) => {
  e.preventDefault()
  isDragOver.value = false
  if (e.dataTransfer && e.dataTransfer.files && e.dataTransfer.files[0]) {
    const file = e.dataTransfer.files[0]
    if (file.type.startsWith('image/')) {
      selectedImg.value = URL.createObjectURL(file)
      userFileName.value = file.name
    }
  }
}

const selectSample = (path: string, name: string) => {
  selectedImg.value = path
  userFileName.value = name + '.jpg'
}

const onImageLoaded = () => {
  if (baseCanvasImgRef.value) {
    const w = baseCanvasImgRef.value.naturalWidth
    const h = baseCanvasImgRef.value.naturalHeight
    selectedImgDimensions.value = `${w} × ${h} PX`
  }
}

// Export high quality JPG
const exportJpg = async () => {
  if (!studioCanvasRef.value) return
  isExporting.value = true

  try {
    const el = studioCanvasRef.value
    const baseImg = baseCanvasImgRef.value
    const scale = baseImg ? 2 : 1
    const width = (baseImg?.naturalWidth || el.clientWidth) * scale
    const height = (baseImg?.naturalHeight || el.clientHeight) * scale

    const dataUrl = await toJpeg(el, {
      canvasWidth: width,
      canvasHeight: height,
      quality: 0.95
    })

    const cleanName = activePreset.value.name.toLowerCase().replace(/\s+/g, '-')
    const timestamp = Date.now()
    saveAs(dataUrl, `photo-${cleanName}-${timestamp}.jpg`)
  } catch (err) {
    console.error('Export failed:', err)
  } finally {
    isExporting.value = false
  }
}

// Keyboard navigation
const cyclePreset = (dir: 1 | -1) => {
  const list = visiblePresets.value
  const currIndex = list.findIndex((p) => p.id === activePresetId.value)
  if (currIndex === -1) {
    applyPreset(list[0])
    return
  }
  let next = currIndex + dir
  if (next < 0) next = list.length - 1
  if (next >= list.length) next = 0
  applyPreset(list[next])
}

const cycleBlend = (dir: 1 | -1) => {
  const currIndex = blendModes.indexOf(globalBlendMode.value)
  let next = currIndex + dir
  if (next < 0) next = blendModes.length - 1
  if (next >= blendModes.length) next = 0
  globalBlendMode.value = blendModes[next]
}

const onKeyDown = (e: KeyboardEvent) => {
  if (e.target instanceof HTMLInputElement || e.target instanceof HTMLTextAreaElement) {
    return
  }
  if (e.key === 'z' || e.key === 'Z') {
    cyclePreset(-1)
  } else if (e.key === 'x' || e.key === 'X') {
    cyclePreset(1)
  } else if (e.key === 'ArrowLeft') {
    cycleBlend(-1)
  } else if (e.key === 'ArrowRight') {
    cycleBlend(1)
  } else if (e.key === 'Escape') {
    resetAll()
  }
}

onMounted(() => {
  window.addEventListener('keydown', onKeyDown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', onKeyDown)
})
</script>

<template>
  <div class="raster-container">
    <!-- Top Action Bar / Section Header -->
    <div class="raster-section-header">
      <span class="raster-index">[ 01 ]</span>
      <h2 class="raster-h2">Studio Darkroom</h2>
      <div class="raster-rule"></div>
      <div class="raster-flex raster-items-center gap-2">
        <div class="raster-btn-group">
          <button
            @click="viewMode = 'studio'"
            class="raster-btn"
            :class="{ 'is-active': viewMode === 'studio' }"
          >
            [ STUDIO VIEW ]
          </button>
          <button
            @click="viewMode = 'gallery'"
            class="raster-btn"
            :class="{ 'is-active': viewMode === 'gallery' }"
          >
            [ GALLERY GRID ]
          </button>
        </div>

        <button
          @click="triggerFileInput"
          class="raster-btn raster-btn-primary raster-btn-sm"
        >
          <span>&uarr;</span> DROP / UPLOAD IMAGE
        </button>
        <input
          ref="fileInput"
          type="file"
          accept="image/*"
          @change="onFileSelected"
          class="d-none"
        />
      </div>
    </div>

    <!-- VIEW MODE 1: STUDIO (Clean, single-image focused Swiss darkroom) -->
    <div v-if="viewMode === 'studio'" class="raster-grid">
      <!-- Main Stage Canvas (Col 8) -->
      <div class="col-8">
        <!-- Canvas Viewport Frame with Swiss Crosshairs -->
        <div
          class="studio-viewport"
          :class="{
            'is-dragover': isDragOver,
            'is-empty': !selectedImg
          }"
          @dragover="onDragOver"
          @dragleave="onDragLeave"
          @drop="onDrop"
        >
          <!-- 4 Swiss Architectural Crosshair corner marks -->
          <div class="crosshair-tl"></div>
          <div class="crosshair-tr"></div>
          <div class="crosshair-bl"></div>
          <div class="crosshair-br"></div>

          <!-- Active Processed Canvas Container (for capture & export) -->
          <div
            v-if="selectedImg"
            ref="studioCanvas"
            class="studio-canvas-wrapper"
          >
            <!-- Optional Light Leak Overlay -->
            <img
              v-if="activeLeakPath"
              :src="activeLeakPath"
              :style="{ mixBlendMode: globalBlendMode as any }"
              class="studio-leak-overlay"
              alt="Light leak effect"
            />
            <!-- Base User Photo with Active Filter Class -->
            <img
              ref="baseCanvasImgImg"
              :src="selectedImg"
              :class="globalFilterMode"
              class="studio-canvas-image"
              @load="onImageLoaded"
              alt="Studio viewport content"
            />
          </div>

          <!-- Empty Canvas State -->
          <div v-else class="text-center py-12">
            <span class="raster-mono text-muted d-block mb-2">[ + NO MEDIA LOADED + ]</span>
            <p class="raster-lead mb-4">Drop an image here or click below to start editing</p>
            <button @click="triggerFileInput" class="raster-btn raster-btn-primary">
              Select Photo from Disk
            </button>
          </div>
        </div>

        <!-- Studio Bottom Control Panel -->
        <div class="raster-card mt-3">
          <div class="raster-flex raster-items-center raster-justify-between flex-wrap gap-3">
            <!-- Left Actions -->
            <div class="raster-flex raster-items-center gap-2">
              <button
                @click="resetAll"
                class="raster-btn raster-btn-outline raster-mono raster-btn-sm"
                title="Reset all filters and blend modes to untouched original"
              >
                [ RESET ALL ]
              </button>

              <div class="raster-flex raster-items-center gap-1">
                <span class="raster-label d-none-sm">SAMPLES:</span>
                <button
                  v-for="s in sampleImages"
                  :key="s.name"
                  @click="selectSample(s.path, s.name)"
                  class="raster-btn raster-btn-ghost raster-btn-sm raster-mono"
                >
                  {{ s.name }}
                </button>
              </div>
            </div>

            <!-- Middle Controls: Blend Mode & Leak FX -->
            <div class="raster-flex raster-items-center gap-2">
              <div class="raster-flex raster-items-center gap-1">
                <span class="raster-label">BLEND:</span>
                <select
                  v-model="globalBlendMode"
                  class="raster-select raster-mono"
                  style="width: 130px; padding: 3px 6px; font-size: 11px;"
                >
                  <option v-for="b in blendModes" :key="b" :value="b">
                    {{ b.toUpperCase() }}
                  </option>
                </select>
              </div>

              <div v-if="activeLeakPath" class="raster-flex raster-items-center gap-1">
                <span class="raster-tag raster-tag-accent raster-mono">[ LEAK FX ACTIVE ]</span>
                <button
                  @click="activeLeakPath = ''"
                  class="raster-btn raster-btn-ghost raster-btn-sm text-accent"
                  title="Remove leak overlay"
                >
                  &times;
                </button>
              </div>
            </div>

            <!-- Right Actions: Primary Export -->
            <div>
              <button
                @click="exportJpg"
                :disabled="isExporting"
                class="raster-btn raster-btn-accent raster-mono"
              >
                <span>&darr;</span>
                <span>{{ isExporting ? '[ EXPORTING... ]' : '[ EXPORT JPG ]' }}</span>
              </button>
            </div>
          </div>

          <!-- Architectural Metadata Bar -->
          <div class="raster-flex raster-items-center raster-justify-between mt-3 pt-3 border-top raster-micro text-muted">
            <div>
              <span>FILE: <strong class="text-primary raster-mono">{{ userFileName }}</strong></span>
              <span class="ml-3">RES: <strong class="text-primary raster-mono">{{ selectedImgDimensions }}</strong></span>
            </div>
            <div class="d-none-sm raster-mono">
              [Z / X] PRESETS &middot; [&larr; / &rarr;] BLEND &middot; [ESC] RESET
            </div>
          </div>
        </div>
      </div>

      <!-- Right Sidebar: Presets Selector (Col 4) -->
      <div class="col-4">
        <div class="raster-card">
          <!-- Sidebar Header -->
          <div class="raster-card-header">
            <div>
              <div class="raster-label">Preset Library</div>
              <h3 class="raster-card-title mt-1">// {{ activePreset.name }}</h3>
            </div>
            <span class="raster-mono text-muted">
              [ {{ visiblePresets.length }} PRESETS ]
            </span>
          </div>

          <!-- Category Segmented Tabs -->
          <div class="raster-btn-group w-full mb-3" style="display: flex;">
            <button
              @click="activeCategoryTab = 'featured'"
              class="raster-btn"
              style="flex: 1;"
              :class="{ 'is-active': activeCategoryTab === 'featured' }"
            >
              FEATURED
            </button>
            <button
              @click="activeCategoryTab = 'classic'"
              class="raster-btn"
              style="flex: 1;"
              :class="{ 'is-active': activeCategoryTab === 'classic' }"
            >
              CLASSIC
            </button>
            <button
              @click="activeCategoryTab = 'leaks'"
              class="raster-btn"
              style="flex: 1;"
              :class="{ 'is-active': activeCategoryTab === 'leaks' }"
            >
              LEAKS
            </button>
            <button
              @click="activeCategoryTab = 'all'"
              class="raster-btn"
              style="flex: 1;"
              :class="{ 'is-active': activeCategoryTab === 'all' }"
            >
              ALL
            </button>
          </div>

          <!-- Search filter box -->
          <div class="mb-3">
            <input
              v-model="presetSearchQuery"
              type="text"
              placeholder="// FILTER BY NAME..."
              class="raster-input raster-mono"
              style="font-size: 11px; padding: 4px 8px;"
            />
          </div>

          <!-- Presets Scroll Container -->
          <div class="presets-scroll-list">
            <div
              v-for="preset in visiblePresets"
              :key="preset.id"
              class="raster-card raster-card-interactive preset-item mb-2"
              :class="{
                'is-active-preset': activePresetId === preset.id
              }"
              @click="applyPreset(preset)"
            >
              <div class="raster-flex raster-items-center raster-justify-between">
                <div class="raster-flex raster-items-center gap-2">
                  <span class="raster-mono text-muted">[ {{ preset.index }} ]</span>
                  <div class="preset-name">{{ preset.name }}</div>
                </div>
                <span
                  v-if="activePresetId === preset.id"
                  class="raster-tag raster-tag-accent raster-mono"
                >
                  ACTIVE
                </span>
              </div>
              <div class="raster-micro text-muted mt-1 truncate">
                {{ preset.subtitle }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- VIEW MODE 2: GALLERY GRID (Full comparison matrix) -->
    <div v-else class="gallery-view-mode">
      <div class="raster-callout mb-6">
        <span class="raster-label">Gallery Comparison Mode</span>
        <p class="raster-small">
          Inspect your photo across all 42 classic photographic profiles and 15 multi-directional light leaks
          rendered simultaneously. Click any card's export button for an instant single-image download.
        </p>
      </div>

      <!-- Basic Filters Gallery Section -->
      <section class="gallery-section mb-8">
        <div class="raster-section-header">
          <span class="raster-index">[ 02.1 ]</span>
          <h3 class="raster-h3">Classic Film Tone Profiles</h3>
          <div class="raster-rule"></div>
          <span class="raster-meta">42 FILTERS LOADED</span>
        </div>
        <div class="gallery-row-scroll">
          <PhotoBasicFilter
            v-for="name in rawFilterNames"
            :key="name"
            :filterName="name"
            :userImage="selectedImg"
          />
        </div>
      </section>

      <!-- Multi-directional leaks section -->
      <section class="gallery-section mb-8">
        <div class="raster-section-header">
          <span class="raster-index">[ 02.2 ]</span>
          <h3 class="raster-h3">Multi-Directional Light Leak Profiles</h3>
          <div class="raster-rule"></div>
          <span class="raster-meta">15 ANALOG LEAKS</span>
        </div>
        <div class="gallery-row-scroll">
          <PhotoFrame
            v-for="n in filterMultiOptions"
            :key="'multi-' + n"
            :filterImage="n"
            filterPath="/lightleak1/Multi-"
            :userImage="selectedImg"
            :globalBlendMode="globalBlendMode"
            :globalFilterMode="globalFilterMode"
          />
        </div>
      </section>

      <!-- Directional leak series -->
      <section
        v-for="series in 7"
        :key="'series-' + series"
        class="gallery-section mb-8"
      >
        <div class="raster-section-header">
          <span class="raster-index">[ 02.{{ series + 2 }} ]</span>
          <h3 class="raster-h3">Light Leak Series {{ series }}</h3>
          <div class="raster-rule"></div>
          <span class="raster-meta">5 CHROMATIC VARIANTS</span>
        </div>
        <div class="gallery-row-scroll">
          <PhotoFrame
            v-for="col in lightLeakColors"
            :key="'series-' + series + '-' + col"
            :filterImage="col"
            :filterPath="'/Light Leak ' + series + '/Light Leak ' + series + ' - '"
            :userImage="selectedImg"
            :globalBlendMode="globalBlendMode"
            :globalFilterMode="globalFilterMode"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
.d-none {
  display: none;
}

.ml-3 {
  margin-left: var(--raster-space-3);
}

.presets-scroll-list {
  max-height: 520px;
  overflow-y: auto;
  padding-right: 4px;
}

.preset-item {
  padding: var(--raster-space-3);
  transition: all 0.15s ease;
  border-left: 3px solid transparent;
}

.preset-item:hover {
  border-color: var(--raster-border-strong);
}

.preset-item.is-active-preset {
  border-left: 3px solid var(--raster-accent);
  background-color: var(--raster-surface-subtle);
}

.preset-name {
  font-weight: 700;
  font-size: var(--raster-text-small);
  letter-spacing: -0.01em;
}

.gallery-row-scroll {
  display: flex;
  flex-wrap: nowrap;
  gap: var(--raster-space-4);
  overflow-x: auto;
  padding-bottom: var(--raster-space-4);
  scroll-snap-type: x mandatory;
}

.gallery-row-scroll > * {
  scroll-snap-align: start;
  flex-shrink: 0;
}

@media (max-width: 768px) {
  .d-none-sm {
    display: none;
  }
}
</style>
