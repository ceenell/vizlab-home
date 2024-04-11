<template v-slot:figures>
  <div class="maxWidth carouselContainer">
    <Carousel 
      class="image-slider"
      :breakpoints="breakpoints"
      v-bind="settings"
    >
      <Slide
        v-for="chart in charts"
        :id="`chart-challenge-${chart.id}`"
        :key="chart"
        class="slide"
      >
        <div class="slider-image-container">
          <picture>
            <source
              :srcset="getImgUrl(chart.image_basename, 'webp')"
              type="image/webp"
            >
            <source
              :srcset="getImgUrl(chart.image_basename, chart.image_type)"
              type="image/jpg"
            >
            <img 
              :id="`chart-img-${chart.id}`"
              class="sliderImage"
              :src="getImgUrl(chart.image_basename, chart.image_type)"
              :alt="chart.image_alt"
              loading="lazy"
              @load="imageLoaded($event, chart.id)"
            >
          </picture>
        </div>
      </Slide>
      <template #addons>
      <Navigation />
    </template>
    </Carousel>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import { Carousel, Navigation, Slide } from 'vue3-carousel'
import CC23 from "@/assets/content/CC23.js";
import 'vue3-carousel/dist/carousel.css'

export default defineComponent({
  name: 'ChartChallenge23Carousel',
  components: {
    Carousel,
    Slide,
    Navigation,
  },
  data: () => ({
    charts: CC23.chartChallengeCharts.sort((a,b) => new Date(a.date) - new Date(b.date)),
    // carousel settings
    settings: {
      itemsToShow: 4,
      snapAlign: 'center',
    },
    // breakpoints are mobile first
    // any settings not specified will fallback to the carousel settings
    breakpoints: {
      // 700px and up
      700: {
        itemsToShow: 3.5,
        snapAlign: 'center',
      },
      // 1024 and up
      1024: {
        itemsToShow: 5,
        snapAlign: 'start',
      },
    },
  }),
  methods:{
    getImgUrl(pic, extension) {
            // TODO: alternative if image is not given or broken
            return 'https://labs.waterdata.usgs.gov/visualizations/charts/' + pic + '.' + extension
          },
          imageLoaded(event, chartId) {
            const image = event.target;
            const width = image.naturalWidth; // Get the natural width of the image
            const slideElement = document.getElementById(`chart-challenge-${chartId}`);
            if (slideElement) {
              slideElement.style.width = `${width}px`; // Adjust the slide's width to match the image
            }
          }
  }
})

</script>

<style scoped lang="scss">
  .carouselContainer {
    max-width: 98%;
  }
  .image-slider {
    margin: auto;
    max-width: 100%;
    *:focus{
      outline: none;
    }
  }
  .slide {
    margin: 0;
    padding: 5px;
    select:focus{
      outline: none;
    }
  }
  .slide:hover {
    transform: translate3D(0,-0.5px,0) scale(1.05);
    transition: all .3s ease; 
  }
  .slider-image-container {
    padding: 10px;
    display: grid;
    grid-template-columns: max-content;
    height: 200px;
    max-width: 275px;
    align-content: center;
    justify-content: center;
    img {
      max-width: 255px;
      max-height: 180px;
    }
  }
  </style>
  <style lang="scss">
  .VueCarousel-dot-container {
    margin-top: 0px !important;
  }
  .VueCarousel-dot {
    margin-top: 0px !important;
  }
  .VueCarousel-navigation {
    @media (max-width: 600px) {
      display: none;
    }
  }
  //Hides v-img title element
  .title-v-img{
    display: none;
  }
  .fullscreen-v-img{
    position: relative;
  }
  .footer-v-img {
    justify-content: start !important;
  }
  .footer-v-img img {
    width: auto !important;
    height: 40px !important;
  }
  #captionArea{
    background: rgba(255,255,255,1);
    position: absolute;
    width: 100%;
    bottom: 0px;
    z-index: 9000;
    text-align: left;
    padding: 20px 10px;
    color: black;
    font-size: 1em;
    display: flex;
    justify-content: center;
    @media screen and (max-width: 600px) {
      padding: 20px 10px;
      font-size: .8em;
    }
    .caption{
      margin: 0 auto 70px auto;
      max-width: 900px;
      line-height: 1.5em;
    }
  }
</style>