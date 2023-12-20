<template>
  <LoaderComponent
    @isAnimationDone="handleIntroduction"
    @isPageReady="handlePage"
  />
  <div class="main" v-show="isPageReady">
    <div class="main__container section" id="main">
      <div class="main__slider__bg" ref="sliderBg">
        <div class="main__slider" ref="slider">
          <article class="main__logo" v-show="isIntroductionDone">
            <h2 class="loader__title loader__title__top">Respect</h2>
            <h2 class="loader__title loader__title__bottom">
              <span class="loader__title__bottom__dot">.</span>Studio
            </h2>
          </article>
          <article class="main__description">
            <p>Digital Marketing</p>
            <p>Agency</p>
          </article>
        </div>
      </div>
      <header class="main__header">
        <img src="@/assets/logo.svg" alt="" />
        <nav class="main__header__nav">
          <a href="">Services</a>
          <a href="">Case Studies</a>
          <a href="">Blog</a>
          <a href="">About us</a>
        </nav>
        <nav class="main__header__nav">
          <a href="">We're hiring</a>
          <a href="">Contacts</a>
        </nav>
        <button class="main__header__button">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="22"
            height="13"
            viewBox="0 0 22 13"
            fill="none"
          >
            <path
              d="M21.1007 6.49956L15.3873 0.786133H12.5306L17.2399 5.49546L0.899353 5.48769L0.900767 7.51284L17.2498 7.51355L12.5306 12.213L15.3866 12.2137L21.1007 6.49956Z"
              fill="#E63E3A"
            />
          </svg>
          Book a call
        </button>
      </header>
    </div>
    <section class="info section" id="info">
      <h1 class="info__title">
        
      </h1>
      <div class="info__description">
        <p class="info__about">B2B Marketing & LinkedIn Lead</p>
        <p class="info__about">Generation agency</p>
      </div>
    </section>
    <GrowthChartComponent :animation="currentAnimation"/>
    <ResultChartComponent :animation="currentAnimation"/>
  </div>
</template>

<script setup>
import LoaderComponent from "@/components/LoaderComponent.vue";
import GrowthChartComponent from "./GrowthChartComponent.vue";
import ResultChartComponent from "./ResultChartComponent.vue";
import { ref, onMounted, onUnmounted, watch } from "vue";
import { gsap, ScrollToPlugin } from "gsap/all";

gsap.registerPlugin(ScrollToPlugin);


const isIntroductionDone = ref();
const isPageReady = ref(false);

const slider = ref();
const sliderBg = ref();

const currentElement = ref(0);

const currentAnimation = ref('');

watch(currentAnimation, (newVal) => {
  if (newVal === 'info') {
    startTextAnimation();
  }
});

onMounted(() => {
  window.addEventListener("wheel", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("wheel", handleScroll);
});

function handleScroll(event) {
  if (isIntroductionDone.value) {
    const sections = document.querySelectorAll(".section");
    for (let i = 0; i < sections.length; i++) {
      if (i === currentElement.value) {
        if (event.wheelDelta < 0 && i !== sections.length - 1) {
          currentAnimation.value = sections[i+1].id
          const offsetY = window.innerHeight - sections[i+1].offsetHeight;
          gsap.to(window, { duration: 1, scrollTo: { y: sections[i + 1], offsetY: offsetY }});
          currentElement.value = i+1;
        } else if (event.wheelDelta > 0 && i !== 0) {
          currentAnimation.value = sections[i-1].id
          const offsetY = window.innerHeight - sections[i-1].offsetHeight;
          gsap.to(window, { duration: 1, scrollTo: { y: sections[i - 1], offsetY: offsetY }});
          currentElement.value = i-1;
        }
        i = sections.length;
      }
    }
  }
}

function handleIntroduction(val) {
  isIntroductionDone.value = val;
  startSliderAnimation();
}

function handlePage(val) {
  isPageReady.value = val;
}

function startSliderAnimation() {
  const interval1 = setInterval(() => {
    sliderBg.value.style.transform = "scale(-1, 1)";
    slider.value.style.transform = "scale(-1, 1)";
  }, 500);
  const interval2 = setInterval(() => {
    sliderBg.value.style.transform = "none";
    slider.value.style.transform = "none";
  }, 1000);
  setTimeout(() => {
    clearInterval(interval1);
    clearInterval(interval2);
  }, 4000);
}

function startTextAnimation() {
  const tl = gsap.timeline();
  tl
  .to(".info__title", {duration: 2.5, text: 'Growing businesses by building relationships'})
  .to(".info__description", {opacity: 100}, "<+=0.5")
}

</script>

<style scoped lang="scss">
.main {
  &__slider {
    padding: 20px;
    display: flex;
    justify-content: space-between;
    &__bg {
      background-size: cover;
      background-image: url("@/assets/bg_photo.jpg");
      height: 100%;
    }
  }
  &__container {
    height: 100vh;
    display: flex;
    flex-direction: column;
  }
  &__logo {
    color: $red-500;
  }
  &__description {
    color: $red-500;
    font-size: 18px;
    text-align: right;
    text-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    margin-left: auto;
  }
  &__header {
    height: 90px;
    padding: 20px;
    width: 100%;
    background-color: $red-500;
    display: flex;
    align-items: center;
    justify-content: space-between;
    &__nav {
      display: flex;
      gap: 60px;
      font-size: 18px;
      a {
        color: $black-600;
      }
    }
    &__button {
      font-size: 20px;
      padding: 12px 18px 14px 115px;
      background-color: $black-600;
      color: $red-500;
      border: none;
      outline: none;
      cursor: pointer;
      svg {
        margin-right: 65px;
        position: relative;
        right: 0;
        transition: right 0.5s;
      }
      &:hover {
        svg {
          right: -50px;
        }
      }
    }
  }
}

.info {
    height: calc(100vh - 90px);
    background-color: $red-500;
    padding: 75px 20px 30px;
    color: $black-600;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-end;
    &__title {
      font-size: 135px;
      text-align: end;
      max-width: 800px;
    }
    &__description {
      opacity: 0;
      transition: opacity 2s;;
    }
    &__about {
      font-size: 22px;
    }
    div {
      align-self: flex-start;
    }
  }
</style>
