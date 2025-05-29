<script setup>

import { ref } from 'vue';



const sites = [
  { txtColor:"black", bgColor:"white", name:"creative direction", url: "https://home.ssome.how/" },
  { txtColor:"white", bgColor:"black", name:"curation", url: "https://www.ssome.how/" },
  { txtColor:"black", bgColor:"white", name:"art & publishing", url: "https://www.j-uk.art/" },
  { txtColor:"white", bgColor:"#181818", name:"design", url: "https://www.offline.li/" },
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
        :style=" { backgroundColor:site.bgColor, width: cardWidths[index] + '%' }"
      ref="cardRefs"
      >
      <div>
        <a :style="{color:site.txtColor}" :href="site.url">{{site.name}}</a>
      </div>
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
  height: 96%;
  width: 25%;
  margin: 7px 3px 5px 3px;
  background-color: green;
  border-radius: 8px;
  padding: 2px;
  filter: drop-shadow(1px 0px 2px #cfcaff);
}

a {
  display: block;
    width: 25vw;
    text-align: center;
    /* color: white; */
    text-decoration: none;
    font-family:  Helvetica Neue, Helvetica, Arial, sans-serif;
    font-weight: 400;
    font-size: 9px;
    margin-top: 6px;
    margin-bottom: 8px;
}


.window {
  width: 100%;
  height: 91%;
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
