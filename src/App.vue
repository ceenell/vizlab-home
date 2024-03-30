<template>
  <div id="app">
    <WindowSize v-if="typeOfEnv === '-test build-'" />
    <HeaderUSWDSBanner v-if="typeOfEnv !== '-test build-'" />
    <HeaderUSGS />
    <WorkInProgressWarning v-if="typeOfEnv === '-beta build-'" />
    <RouterView />
    <FooterUSGS />
  </div>
</template>

<script setup>
  import { onMounted } from "vue";
  import { RouterView } from 'vue-router'
  import WindowSize from "./components/WindowSize.vue";
  import HeaderUSWDSBanner from "./components/HeaderUSWDSBanner.vue";
  import HeaderUSGS from './components/HeaderUSGS.vue';
  import WorkInProgressWarning from "./components/WorkInProgressWarning.vue";
  import FooterUSGS from './components/FooterUSGS.vue';
  import { useWindowSizeStore } from './stores/WindowSizeStore';

  const windowSizeStore = useWindowSizeStore();
  const typeOfEnv = import.meta.env.VITE_APP_TIER;

  // Declare behavior on mounted
  // functions called here
  onMounted(() => {
    // Add window size tracking by adding a listener
    window.addEventListener('resize', handleResize);
    handleResize();
  });

  // Functions
  function handleResize() {
    // store the window size values in the Pinia state
    windowSizeStore.windowWidth = window.innerWidth;
    windowSizeStore.windowHeight = window.innerHeight;
  }
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,200;1,300;1,400;1,600;1,700;1,900&display=swap');
$nearBlack: #181a1a;
$coolBlue: rgb(66, 145, 235);
$sourceSans: 'Source Sans Pro', sans-serif;

.logo-header {
  margin-left: 0.5rem;
}
html,
body {
      height:100%;
      margin: 0;
      padding: 0;
      line-height: 1.2;
      font-family: $sourceSans;
      font-size: 16px;
      font-weight: 400;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      width: 100%;
      @media screen and (max-width: 600px) {
        font-size: 16px;
      }
  }
  h1{
  font-size: 3.5em;
  font-weight: 600;
  font-family: $sourceSans;
  line-height: 1;
  text-align: left;
  @media screen and (max-width: 600px) {
    font-size: 2.5em;
  }
}
h2{
  font-weight: 600;
  text-align: left;
  color: white;
  font-family:$sourceSans;
  font-size: 2.5em;
  margin-top: 2px;
  line-height: 1;
  @media screen and (max-width: 600px) {
    font-size: 2em;
  }
  padding-left: 0rem;
}
h3{
  font-size: 1.5em;
  padding-top: 0em;
  color: white;
  margin: 0px;
  font-family: $sourceSans;
  font-weight: 600;
  @media screen and (max-width: 600px) {
      font-size: 1em;
  }  
}
h4{
  font-size: 1.2em;
  padding: 0.3em 0em 0.2em 0em;
  margin: 0px;
  font-family: $sourceSans;
  font-weight: 400;
  @media screen and (max-width: 600px) {
      font-size: 1em;
  }  
}
a {
  color: $coolBlue;
}
.site-preview-heading {
  padding: 1rem;
}
$theme-accordion-font-family: $sourceSans;
p {
  font-size: 1.2rem;
}
.button.usa-accordion__button {
  font-size: 3rem;
}
</style>
