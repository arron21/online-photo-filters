<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  filterName: string
}>()

const displayName = computed(() => {
  return props.filterName.replace(/^filter-/, '').toUpperCase()
})
</script>

<template>
  <div class="raster-label raster-mono">
    {{ displayName }}
  </div>
</template>
