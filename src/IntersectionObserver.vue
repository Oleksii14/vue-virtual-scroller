<script lang="ts" setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'

const emit = defineEmits(['intersected'])

const observerRef = ref()
const observerInstance = ref()

onMounted(() => {
  observerInstance.value = new window.IntersectionObserver(([entry]) => {
    if (entry?.isIntersecting) {
      emit('intersected')
    }
  })

  if (observerRef.value) {
    observerInstance.value.observe(observerRef.value)
  }
})

onBeforeUnmount(() => observerInstance.value?.disconnect())
</script>

<template>
  <div ref="observerRef" style="height: 1px" />
</template>
