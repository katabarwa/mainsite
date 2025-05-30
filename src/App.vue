<script setup>

import { ref, computed, onMounted, onBeforeUnmount, watch, onUnmounted } from 'vue';

import { useScreenOrientation } from '@vueuse/core'




const sites = [
  { txtColor:"black", bgColor:"white", name:"creative direction", url: "https://home.ssome.how/" },
  { txtColor:"white", bgColor:"black", name:"curation", url: "https://www.ssome.how/" },
  { txtColor:"black", bgColor:"white", name:"art & publishing", url: "https://www.j-uk.art/" },
  { txtColor:"white", bgColor:"#181818", name:"design", url: "https://www.offline.li/" },
];


const cardWidths = ref(Array(sites.length).fill(100 / sites.length));

let portrait = window.matchMedia("(orientation: portrait)");

const isPortrait = ref(portrait.matches)
console.log("Initial orientation:", portrait.matches)

portrait.addEventListener("change", function(e) {
    if(e.matches) {
        isPortrait.value = true
        console.log("portrait")   
    } else {
        isPortrait.value = false
        console.log("landscape")
    }
    console.log("isPortrait",isPortrait.value)
    window.location.reload()
})


const {
  isSupported,
  orientation,
  angle,
  lockOrientation,
  unlockOrientation,
} = useScreenOrientation()


console.log("screen orientation",useScreenOrientation().orientation.value)

watch(isPortrait, () => {
  console.log("Orientation changed:", isPortrait.value)
  applyBodyStyles()
})

onMounted(() => {
  console.log("Initial orientation:", orientation.value)
  applyBodyStyles()
})

const applyBodyStyles = () => {
  const body = document.body;
  const main = document.querySelector('main');
  const cards = document.getElementsByClassName("card");
  const h2 = document.querySelectorAll('h2');
  const iframes = document.querySelectorAll('iframe');
  
  if (isPortrait.value) {
    body.style.overflowY = 'scroll'
    body.style.overflowX = 'hidden'
    main.style.flexDirection = 'column'
    main.style.width = '100%'
    main.style.height = '250%'
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
    main.style.height = '95%'
    
    for (let i=0; i < cards.length; i++) {
      cards[i].style.width = '24%'
      cards[i].style.height = '99%'
      cards[i].style.marginBottom = '10px'
      cards[i].style.display = 'flex'
      cards[i].style.flexDirection = 'column'
      cards[i].style.alignItems = 'center'
      h2[i].style.width = '100%'
      iframes[i].style.width = '98%'
      iframes[i].style.height = '92%'
      iframes[i].style.marginBottom = '10px'
    }
  }
  
  body.style.transition = 'all 0.5s ease'
}


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

  <footer>

  <div class="contact">
    <h2>
      <a href="mailto: jesse@ssome.how">Email</a>
      <a href="mailto: jesse@ssome.how">Instagram</a>
    </h2>
  </div>
  <div class="legal">
     <h2>2025 © Copyright  <br>
      All Rights Reserved <br>Jesse Katabarwa</h2>
     <!-- <h2>This website and it's content were designed and developed by Jesse Katabarwa</h2> -->
  </div>

  </footer>
</template>

<style >
@media screen and (max-width: 580px) {
h1 a {
  letter-spacing: 25px !important;
  background-color: #e9e9e9 !important;
  border-radius: 10px;
 

}
 h1 {
  background-color: #f4f4f4 !important;
  border-radius: 10px;

}

}
.legal h2 {
  font-size: 7px;
  font-weight: 400;
  padding: 0 10px 5px 10px ;
  text-align: center;
  margin-top: 8px;
  text-transform: uppercase;


}

.contact h2 {
  display: flex;
  flex-direction: column;
justify-content: space-between;
text-transform: uppercase;

}



.contact h2 a {
  color: black;
  font-size: 7px;
  padding: 0 10px 0 10px ;
  cursor: pointer;

}

.contact h2 a:hover{
  letter-spacing: 0.3px;;
}

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

body {
  font-family: 'Helvetica Neue', Helvetica, Helvetica system-ui, -apple-system, BlinkMacSystemFont, 'Helvetica Neue', Arial , sans-serif;

}


h1, h1 a{
  font-size: 7px;
  font-weight: 400;
  letter-spacing: 30px;
  text-align: center;
  cursor: pointer;
  margin: 0;
  text-indent: 30px;
  text-decoration: none;
  color: grey;
}

h1 a:hover {
  letter-spacing: 35px;
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



</style>
