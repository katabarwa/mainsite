<script setup>

import { ref, computed, onMounted, onBeforeUnmount, watch, onUnmounted } from 'vue';



const sites = [
  { txtColor:"black", bgColor:"white", name:"creative direction", url: "https://home.ssome.how/" },
  { txtColor:"white", bgColor:"black", name:"curation", url: "https://www.ssome.how/" },
  { txtColor:"black", bgColor:"white", name:"art & publishing", url: "https://www.j-uk.art/" },
  { txtColor:"white", bgColor:"#181818", name:"design", url: "https://www.offline.li/" },
];


const cardWidths = ref(Array(sites.length).fill(100 / sites.length));

const windowWidth = ref(window.innerWidth)
const windowHeight = ref(window.innerHeight)
const supportsOrientation = 'orientation' in window.screen;

console.log("asd",window.screen.orientation.type)

const isPortrait = computed(() => {
  if (supportsOrientation) {
    return window.screen.orientation.type.includes('portrait');
  } else {
    return windowHeight.value > windowWidth.value;
  }
});

function updateOrientation() {
  windowWidth.value = window.innerWidth;
  windowHeight.value = window.innerHeight;
}

const applyBodyStyles = () => {
  const body = document.body;
  const main = document.querySelector('main');
  const cards = document.getElementsByClassName("card");
  const h2 = document.querySelectorAll('h2');
  const iframes = document.querySelectorAll('iframe');
  

  
  if (isPortrait.value) {
    
    body.style.overflow = 'scroll'
    main.style.flexDirection = 'column'
    main.style.width = '100%'
    main.style.height = '400%'
    for (let i=0; i < cards.length; i++) {
      cards[i].style.height = '70vh'
      cards[i].style.paddingBottom = '10px'
      cards[i].style.display = 'flex'
      cards[i].style.flexDirection = 'column'
      cards[i].style.alignItems = 'center'


    }
    for (let i=0; i < cards.length; i++) {
      

      iframes[i].style.width = '97.4%'
      iframes[i].style.height = '90%'
      iframes[i].style.paddingBottom = '10px'
    }
    h2.style.width = '100%'




    } else {
   
        main.style.flexDirection = 'row'
        main.style.width = '100%'
        main.style.height = '80%'
      
        
     
        for (let i=0; i < cards.length; i++) {
          cards[i].style.width = '24%'
          cards[i].style.height = 'fit-content'
          cards[i].style.marginBottom = '10px'
          h2[i].style.width = '100%%'
          iframes[i].style.width = '98%'
          iframes[i].style.height = 'fit-content'
          iframes[i].style.marginBottom = '10px'
       };
       for (let i=0; i < cards.length; i++) {
       
     
    };
    }
  
  body.style.transition = 'all 0.5s ease'
}

const originalStyles = ref({})

watch(isPortrait, () => {
  applyBodyStyles()
  updateOrientation()
})

onMounted(() => {


  window.addEventListener('orientationchange', updateOrientation);
  window.addEventListener('resize', updateOrientation);

  applyBodyStyles()
})

onUnmounted(() => {
  // Remove resize listener
  window.removeEventListener('orientationchange', updateOrientation);
  window.removeEventListener('resize', updateOrientation);
  
})








</script>

<template>

  <header>
    <div class="head">
      
        <h1><a href="mailto: jesse@ssome.how" >JESSE KATABARWA   </a></h1>
   

    </div>


  </header>

  <main>
    <div
      v-for="site in sites"
      :key="site"
      class="card"
        :style=" { backgroundColor:site.bgColor, width: cardWidths[index] + '%' }"
      ref="cardRefs"
      >
      <h2>
        <a  :style="{color:site.txtColor}" :href="site.url">{{site.name}}</a>
      </h2>
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

<style >

main {
  display: flex;
}


header {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: 4px;
}

.head {

  width: fit-content;
  height: fit-content;
  background-color: #dedcdc;
  margin: 7px 0 8px 0;
  border-radius: 15px;


}

.head:hover {
  background-color: #f4f4f4;
}

h1, h1 a{
  font-family: 'Helvetica Neue', Helvetica, Helvetica system-ui, -apple-system, BlinkMacSystemFont, 'Helvetica Neue', Arial , sans-serif;
  font-size: 7px;
  font-weight: 400;
  letter-spacing: 40px;
  text-align: center;
  cursor: pointer;
  margin: 0;
  text-indent: 43px;
  text-decoration: none;
  color: grey;
}

h1 a:hover {
  letter-spacing: 45px;
} 



.card {
  position: relative;
  min-width: 10%;
  margin: 6px 3px 5px 3px;
  background-color: green;
  border-radius: 8px;
  padding: 2px;
  filter: drop-shadow(1px 0px 2px #cfcaff);
}

h2 a {
  display: block;
 
    text-align: center;
    text-decoration: none;
    font-family:  Helvetica Neue, Helvetica, Arial, sans-serif;
    font-weight: 400;
    font-size: 9px;
    margin-top: 6px;
    margin-bottom: 8px;
}


.window {

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
