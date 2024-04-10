<template>
  <div id="visualization">
    <section id="viz-header">
      <vizHeader id="viz-header-svg" />
    </section>
    <section id="viz-menu">
      <ContentHeader id="sticky-header" />
    </section>
    <div id="sticky-body">
      <section id="viz-new">
        <WhatsNew />
      </section>
      <section id="viz-cards">
        <PortfolioAccordions />
      </section>
      <section id="viz-about">
        <About />
      </section>
      <!--     <section id="viz-follow">
      <FollowUs />
    </section> -->
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUpdated, ref } from 'vue';
import vizHeader from "@/assets/usgsHeaderAndFooter/viz-header.svg"; 
import ContentHeader from '@/components/ContentHeader.vue';
import About from '@/components/About.vue';
import WhatsNew from '@/components/WhatsNew.vue';
import PortfolioAccordions from '@/components/PortfolioAccordions.vue';

const header = ref(null);
const sticky = ref(0);

// sticky nav bar
const stickyOnScroll = () => {
  if (window.pageYOffset > sticky.value) {
    header.value.classList.add("sticky");
  } else {
    header.value.classList.remove("sticky");
  }
};

onMounted(() => {
  window.onscroll = stickyOnScroll;
  header.value = document.getElementById("viz-menu");
  sticky.value = header.value.offsetTop;
  // Initialize any other mounted logic here
  findCarouselContainers();
});

onUpdated(() => {
  // Implement logic that needs to run after every update
  lazyLoadImages();
});
   

const lazyLoadImages = () => {
  const loadImg = (entries, observer) => {
    entries.forEach(entry => {
      if(entry.isIntersecting){
        entry.target.srcset = entry.target.dataset.srcset; 
        entry.target.nextElementSibling.srcset = entry.target.dataset.srcset;
        const findImg = entry.target.parentElement.querySelector("img");
        findImg.onload = () => {
          entry.target.parentElement.classList.remove('lazy');
        };
        observer.unobserve(entry.target);
      }
    });
  }
  const imgTargets = document.querySelectorAll(".lazy > source");
  const imgObserver = new IntersectionObserver(loadImg, {
    root: null,
    threshold: 0,
    rootMargin: "300px"
  });
  imgTargets.forEach(img => {
    imgObserver.observe(img);
  });
};

const findCarouselContainers = () => {
  const carouselContainers = document.querySelectorAll(".carouselContainer");
  carouselContainers.forEach(container => {
    container.addEventListener("click", addFooterCaption);
  });
};

const addFooterCaption = (e) => {
  const imgContainer = document.querySelector(".fullscreen-v-img");
  const title = document.querySelector(".title-v-img");
  const img = document.querySelector(".content-v-img img");

  const observer = new MutationObserver((changes) => {
    changes.forEach(change => {
      if(change.attributeName.includes('src')){
        switchCaptionText(title);
      }
    });
  });
  observer.observe(img, {attributes: true});

  if(e.target.classList.contains("sliderImage")){
    const captionHTML = `<div id="captionArea"><div class="caption">${title.textContent}</div></div>`;
    imgContainer.insertAdjacentHTML("afterbegin", captionHTML);
  }

  img.style.maxHeight = "68vh";
  img.style.top = "-15vh";
};

const switchCaptionText = (text) => {
  const caption = document.querySelector(".caption");
  caption.innerHTML = text.textContent;
};
</script>

<style scoped lang="scss">
$nearBlack: #212222;

// repeating section style
section {
  //margin-bottom: 1rem;
  margin-top: 1rem;
  margin-left: 5%;
}
// create page layout as grid
// mobile
#sticky-body {
  top: 150px;
  margin: 0%;
  margin-top: 0%;
  display: grid;
  width: 100vw;
 // grid-template-columns: 1fr 1fr;
  grid-template-areas:
 // "logo logo"
  //"menu-bar menu-bar"
  "whatsNew whatsNew"
  "cards cards"
  "about about"
  "follow follow"

}
#visualization {
  position: relative;
}
#viz-header {
  grid-area: logo;
  background-color: $nearBlack;
  margin: 0;
    width: 100vw;
}
#viz-menu {
  grid-area: menu-bar;
  display: fixed;
  margin-top: 0;
    margin-left: 0;
}
#viz-cards {
  grid-area: cards;
}
#viz-new {
  grid-area: whatsNew;
}
#viz-about {
  grid-area: about;
}
#viz-follow {
  grid-area: follow;
}

// each section is assigned a grid area
// elements within each section contained in div to control positioning

#viz-header-svg {
  margin-left: 5vw;
  margin-top: 1rem;
  background-color: $nearBlack;
  fill: white;
  //width: 60%;
  max-width: 600px;
}
#viz-header {
  background-color: $nearBlack;
}
// desktop layout
@media (min-width:1024px) {
  #sticky-body {
    grid-template-areas:
    //"logo logo"
    //"menu-bar menu-bar"
    "whatsNew whatsNew"
    "cards cards"
    "about about"
   "follow follow"
  }
#viz-header-svg {
  max-width: 600px;
}
}
@media (max-width: 63.99 em){
  
#viz-header-svg {
  width: 40vw;
  min-width: 400px;
  background-color: $nearBlack;
}
}
.sticky {
    position: sticky;
  position: -webkit-sticky;
  top:0;
  left: 0;
  width: 100vw;
  z-index:50;
}
#viz-menu {
  margin-left: 0;
}
</style>
