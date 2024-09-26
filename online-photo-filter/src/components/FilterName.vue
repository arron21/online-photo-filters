<script setup lang="ts">
import { ref, onMounted } from 'vue'

const displayName = ref()
const props = defineProps<{
  filterName: string
}>()

onMounted(() => {
  displayName.value = props.filterName.split('-')[1]
})

</script>

<template>
  <div class="filter-name">
    {{ displayName }}
  </div>
</template>

<style scoped>
.filter-name {
  font-weight: 500;
}
</style>
