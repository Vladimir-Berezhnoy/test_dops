<template>
  <section
    class="loader__section"
    :class="isReady === true ? 'leave-out' : null"
    :style="[isAnimationEnd === true ? { 'display': 'none'} : null]"
    ref="loader"
  >
    <article class="loader__title__container" v-if="progress >= 100">
      <h2 class="loader__title loader__title__top">
        R<span ref="titleTop"></span>
      </h2>
      <h2 class="loader__title loader__title__bottom">
        <span class="loader__title__bottom__dot">.</span>S<span
          ref="titleBottom"
        ></span>
      </h2>
    </article>
    <article class="loader__progress" v-else>
      <p>
        {{ Math.round(progress) }}
      </p>
    </article>
  </section>
</template>

<script setup>
import { onMounted, ref, watch, defineEmits } from "vue";
import { gsap, TextPlugin } from "gsap/all";

const emit = defineEmits(["isAnimationDone", "isPageReady"]);

const loader = ref();

const titleTop = ref(); // Верхняя часть логотипа
const titleBottom = ref(); // Нижняя часть логотипа

const progress = ref(0); // Текущий прогресс
const loadedImage = ref(0); // Количество загруженных картинок

const isReady = ref(false);
const isAnimationEnd = ref(false)

document.body.style.overflow = "hidden";

watch(progress, (newVal) => {
  if (newVal >= 100) {
    setTimeout(() => {
      emit("isPageReady", true);
      gsap.to(titleTop.value, { duration: 1.2, text: "espect", delay: 0.8 });
      gsap.to(titleBottom.value, { duration: 1.2, text: "tudio", delay: 0.8 });
    });
    setTimeout(() => {
      isReady.value = true;
    }, 2000);
  }
});

onMounted(() => {
  gsap.registerPlugin(TextPlugin);

  // Заглушка
  const imagesCountStub = 8; 
  const percentPerImageStub = 100 / imagesCountStub; 

  for (let i = 0; i < imagesCountStub; i++) {
    setTimeout(() => {
      handleLoad(percentPerImageStub);
    }, 400 * i);
  }

  // Для реального проекта код ниже, а код выше закомментировать 

  // const imagesCount = document.querySelectorAll("img").length; // Общее количество картинок
  // const percentPerImage = 100 / imagesCount; // Сколько % от общего количества занимает одна картинка

  // for (let i = 0; i < imagesCount; i++) {  // Раскомментировать для реального проекта
  //   const img_copy = new Image();
  //   img_copy.scr = document.images[i].src;
  //   img_copy.onload = handleLoad(percentPerImage);
  // }

  

  loader.value.addEventListener("transitionend", (event) => {
    if (event.target !== event.currentTarget) {
      return; // Ignore it
    }
    isAnimationEnd.value = true;
    emit("isAnimationDone", true);
    document.body.style.overflow = "auto";
  });
});

function handleLoad(percent) {
  progress.value += percent;
  loadedImage.value++;
}
</script>

<style lang="scss">
.loader {
  &__section {
    transition: all 1.2s;
    height: 100vh;
    max-height: 100vh;
    width: 100vw;
    background-color: $black-600;
    color: $red-500;
  }
  &__title {
    font-size: 72px;
    &__container {
      padding: 20px;
    }
    &__top {
      position: relative;
    }
    &__bottom {
      writing-mode: vertical-rl;
      position: relative;
      padding-left: 8px;
      &__dot {
        position: absolute;
        left: -10px;
        top: -5px;
      }
    }
  }
  &__progress {
    position: absolute;
    bottom: 5px;
    right: 10px;
    font-size: 220px;
  }
}

.leave-out {
  max-height: 0px;
}
</style>
