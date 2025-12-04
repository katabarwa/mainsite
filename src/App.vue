<script setup>

import { ref, computed, onMounted, onBeforeUnmount, watch, onUnmounted } from 'vue';

import { useScreenOrientation } from '@vueuse/core'




const sites = [
  { txtColor:"black", bgColor:"white", name:"commissions", url: "https://home.ssome.how/" },
  { txtColor:"white", bgColor:"black", name:"research", url: "https://www.ssome.how/" },
  { txtColor:"black", bgColor:"white", name:"art & publishing", url: "https://www.j-uk.art/" },
  { txtColor:"white", bgColor:"black", name:"design", url: "https://www.offline.li/" }
];

const headerSites = [  { txtColor:"white", bgColor:"black", name:"design", url: "https://afila.si/" }]


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

// watch(isPortrait, () => {
//   console.log("Orientation changed:", isPortrait.value)
//   applyBodyStyles()
// })

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
       iframes[4].style.height = '92%'
      iframes[i].style.marginBottom = '10px'
      iframes[4].style.width = '98%'
      iframes[4].style.height = '92%'
      iframes[4].style.marginBottom = '10px'
    }
  }
  
  body.style.transition = 'all 0.5s ease'
}

const isActive = ref(false)
const isActiveAfilasi = ref(false)

const toggleActive = () => {
  if (window.matchMedia('(min-width: 598px)').matches){
    isActive.value = !isActive.value
  }

}

const toggleActiveAfilasi = () => {
  if (window.matchMedia('(min-width: 598px)').matches){
    isActiveAfilasi.value = !isActiveAfilasi.value
  }

}

</script>

<template>

  <header>
   
    <div class="head">
        <h1><a href="mailto: jesse@ssome.how" >JESSE KATABARWA   </a></h1>
    </div>

      <div @click="toggleActive" :class="{ active: isActive }"  class="head-image">
          <div class="recent-header":class="{ active: isActive }">
            <h1>RECENT</h1>
          </div>
          <div :class="{ active: isActive }" class="recent-content">
            <img width="100%" src="/src/assets/ssh_jessekatabarwa01.jpg" alt="Art Work">
            <div class="recent-header">
              <h1 class="recent">The Grey Rings of My Eyes</h1> 
              <h1 class="recent">(2025)</h1> 
            </div>
            <img width="100%" src="/src/assets/ssh_jessekatabarwa.jpg" alt="Art Works">
            <div class="recent-header">
              <h1 class="recent">The Grey Rings of My Eyes</h1> 
              <h1 class="recent">(2025)</h1> 
            </div>
          </div>
      </div>

      <div @click="toggleActiveAfilasi" :class="{ active: isActiveAfilasi }"  class="afilasi">
          <div class="afilasi-header":class="{ active: isActiveAfilasi }">
            <h1>2020 - 2023</h1>
          </div>
          <div :class="{ active: isActive }" class="afilasi-content">
            <iframe :class="{ active: isActiveAfilasi }" 
        :src="headerSites[0].url"
        class="window"
        frameborder="0"
      ></iframe>
          </div>
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

  
  </div>
  

  </main>

  <footer>

  <div class="contact">
    <h2>
      <a href="mailto: jesse@ssome.how">Email</a>
      <a href="https://www.instagram.com/jas.offline/">Instagram</a>
    </h2>
  </div>
  <div class="legal">
     <h2>2025 © Copyright 
      All Rights Reserved Jesse Katabarwa <br>
     This website and it's content were designed and developed by Jesse Katabarwa</h2>
  </div>

  </footer>
</template>

<style >

.afilasi.active {
  z-index: 100;
    position: absolute;
    top: 20%;
    display: flex
;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 3px;
    overflow-y: scroll;
    overflow-x: hidden;
    width: 85%;
    height: auto;
    background-color: #dedcdc;
    padding: 5px 10px;
    border-radius: 10px;
}
.afilasi {
  background-color: #dedcdc;
  margin-top: 2px;
}
.afilasi-content {
  width: 100%;


  
}


.afilasi iframe {
  display: none;
}

.afilasi iframe.active {
  display: block;
  width: 75%;
  height: 50vh !important;;
}

.head-image, .afilasi{
  cursor: pointer;
  transition: all 0.3s ease;
}

.head-image:hover {
  width: 28%;
  top:19%;
}

h1 a {
  transition: all 0.4s ease;
}

.head-image h1, .afilasi h1 {
  transition: all 0.4s ease;
}

.head-image.active {
  position: relative;
  margin: 0;
  padding: 0;
  background-color: #dedcdc;
  
}

.head-image.active:hover, .afilasi:hover{
  background-color: #f4f4f4;
  border-radius: 15px;
}

.head-image.active h1:hover, .afilasi h1:hover{

  letter-spacing: 35px;}

.recent-header.active{
  justify-content: center;
}

.recent-content {
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.recent-content.active{
  display: none;
}

@media screen and (max-width: 580px) {

  .afilasi {
    display: none;
  }

  main, iframe {
    display: none !important;
  }

  .card { 
    display: none !important;
  }

  body{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  header {
    margin-top: 15px !important;
  }
    
  h1 a {

      letter-spacing: 15px !important;
      border-radius: 10px;
      padding: 2px;

    

    }
    h1 {
      padding: 2px;
      background-color: #f4f4f4 !important;
      border-radius: 10px;

    }

    .head-image {
      width: 80% !important;
      overflow-y: scroll;
      overflow-x: hidden;
    }

    main {
      display: none !important;
    }

    footer {
      margin-top: 165%;
      text-align: center;
    }

    footer h2 a {
      font-size: 35px!important;
      color: #acacac !important;
    }

}

footer, footer a {

  color: grey;
}

.legal h2 {
  font-size: 7px;
  font-weight: 400;

  text-align: center;
padding-bottom: 2px;
  text-transform: uppercase;
  


}

.contact h2 {
  display: flex;
  flex-direction: column;
justify-content: space-between;
text-transform: uppercase;

}



.contact h2 a {
  color: grey;
  font-size: 7px;

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
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
 
}

.head-scroll {
  z-index: 100;
  position: absolute;
  top: 0;
  height: 100vh;
  scroll-margin-top: 500px;
  width: 30%;
  padding: 5px;
  overflow-y: scroll;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.head-image {
    z-index: 100;
    position: absolute;
    top: 20%;
    display: flex
;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 3px;
    overflow-y: scroll;
    overflow-x: hidden;
    width: 30%;
    height: auto;
    background-color: #f4f4f4;
    padding: 5px 10px;
    border-radius: 10px;
}

.recent-header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
}

.recent {
  font-style: italic;
  letter-spacing: 0px;
  text-indent: 0px;
}

.head-image img {
  border-radius: 5px;
  margin-top: 6px;
  margin-bottom: 2px;
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

.card h2 {
  margin: 10px;
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
    

}


.window {

  border-radius: 8px;

}



</style>
