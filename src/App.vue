<script setup>

import { ref } from 'vue';


const sites = [
  { url: "https://home.ssome.how/" },
  { url: "https://www.ssome.how/" },
  { url: "https://www.j-uk.art/" },
  { url: "https://www.offline.li/" },
];

// Initialize equal percentage widths
const cardWidths = ref(Array(sites.length).fill(100 / sites.length));
const dashboardRef = ref(null);
console.log("cardWidths",cardWidths.value);
let resizingIndex = null;
let startX = 0;
let containerWidth = 0;

function startResizing(index, event) {
  console.log('startResizing:', index);
  resizingIndex = index;
  startX = event.clientX;
  containerWidth = dashboardRef.value.offsetWidth;

  window.addEventListener('mousemove', handleResize);
  window.addEventListener('mouseup', stopResizing);
}

function handleResize(event) {
  if (resizingIndex === null) return;

  const dx = event.clientX - startX;
  const deltaPercent = (dx / containerWidth) * 100;

  const newLeft = cardWidths.value[resizingIndex] + deltaPercent;
  const newRight = cardWidths.value[resizingIndex + 1] - deltaPercent;
  if (newLeft < 10 || newRight < 10) return;

  cardWidths.value[resizingIndex] = newLeft;
  cardWidths.value[resizingIndex + 1] = newRight;
  startX = event.clientX;
}

function stopResizing() {
  window.removeEventListener('mousemove', handleResize);
  window.removeEventListener('mouseup', stopResizing);
  resizingIndex = null;
}
</script>

<template>

  <header>

  </header>

  <main ref="dashboardRef">
    <div
      v-for="site in sites"
      :key="site"
      class="card"
        :style="{ width: cardWidths[index] + '%' }"
      ref="cardRefs"
      >
      <iframe 
        :src="site.url"
        class="window"
        frameborder="0"
      ></iframe>
      <div
       
        class="resize-handle"
        @mousedown="startResizing(index, $event)"
      ></div>
  
  </div>
  </main>
</template>

<style scoped>


main{
  width: 100%;
  height: 102%;
  display: flex;
  flex-direction: row;
}

.card {
  position: relative;
  min-width: 10%;
  height: 100%;
  width: 25%;
  margin: 5px;

  

}

.window {
  width: 100%;
  height: 100%;
  border-radius: 8px;
}

/* .resize-handle {
  position: absolute;
  right: -4px;
  top: 0;
  width: 4px;
  height: 100%;
  cursor: ew-resize;
  background: rgb(213, 213, 213);
  z-index: 10;
} */

</style>
