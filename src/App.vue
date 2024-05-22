<script setup>
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css'
import { DynamicScroller, DynamicScrollerItem } from 'vue-virtual-scroller'

import IntersectionObserver from './IntersectionObserver.vue'

import { ref } from 'vue'

const items = ref([])
const page = ref(1)

const addItems = () => {
  fetch(`https://randomuser.me/api/?page=${page.value}&results=30`)
    .then((r) => r.json())
    .then((data) => {
      items.value = items.value.concat(
        data.results.map((item, idx) => {
          if (idx === 1) {
            return {
              ...item,
              picture: {
                ...item.picture,
                medium:
                  'https://c8.alamy.com/comp/2AP1J6J/eiffel-tower-in-paris-portrait-orientation-2AP1J6J.jpg'
              }
            }
          }
          return item
        })
      )
    })

  page.value++
}
</script>

<template>
  <div class="container">
    <DynamicScroller
      key-field="phone"
      page-mode
      :items="items"
      :min-item-size="40"
      style="flex-grow: 1"
    >
      <template v-slot="{ item, index, active }">
        <DynamicScrollerItem
          :size-dependencies="[item.phone]"
          :item="item"
          :active="active"
          :data-index="index"
        >
          <div style="display: flex; flex-direction: column">
            {{ item.phone }}

            <img :src="item.picture.medium" />
          </div>
        </DynamicScrollerItem>
      </template>
    </DynamicScroller>
    <IntersectionObserver @intersected="addItems" />
  </div>
</template>

<style>
#app {
  display: flex;
  flex-direction: column;
  height: 100%;
}

body {
  height: calc(100vh - 16px);
}

img {
  width: 200px;
}

.container {
  display: flex;
  flex-grow: 1;
  flex-direction: column-reverse;
  overflow: auto;
}
</style>
