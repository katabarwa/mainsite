<script setup>
const sites = [
  { url: "https://home.ssome.how/" },
  { url: "https://www.ssome.how/" },
  { url: "https://www.j-uk.art/" },
  { url: "https://www.offline.li/" },
];
</script>

<template>
  <header>

  </header>

  <main>
    <div
      v-for="site in sites"
      :key="site.name"
      class="dashboard"
      >
      <iframe
        :src="site.url"
        class="window"
        frameborder="0"
      ></iframe>
  
  </div>
  </main>
</template>

<style scoped>
main{
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
}

.dashboard {
  width: 25%;
  height: 100%;
}

.window {
  width: 100%;
  height: 100%;
}

</style>
