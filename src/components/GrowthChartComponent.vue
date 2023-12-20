<template>
  <section class="chart__section section" id="chart">
    <h3 class="chart__title" id="title">
      
    </h3>
    <p class="chart__description" id="description">
      We combine disruptive marketing techniques with proven tech solutions to
      provide maximum business value.
    </p>
    <article class="chart" 
    :style="[{ 'min-height': chartHeight + 'px'}]"
    >
      <div class="chart__container" v-for="(item, index) in chart" :key="index">
        <p class="chart__value">{{ item }}</p>
        <div :class="`chart__element chart__element__${index}`"></div>
      </div>
    </article>
  </section>
</template>

<script setup>
import { gsap } from "gsap/all";
import { defineProps, watch, computed} from "vue";

const props = defineProps({
  animation: {
    type: String,
  },
});

const chartHeight = computed(() => {
  return Math.max(...chart) * 6 + 30
})

watch(() => props.animation, (newVal) => {
  if (newVal === 'chart') {
    startTextAnimation();
    startChartAnimation();
  }
});

const chart = [27, 53, 61, 58, 30, 63, 55, 63, 71, 76, 67, 88, 78, 90];

function startTextAnimation() {
  const tl = gsap.timeline();
  tl
  .to("#title", {duration: 1.5, text: 'Consitent leads flow to streamline Your business growth.'})
  .to("#description", {opacity: 100}, "<+=0.5")
}

function startChartAnimation() {
  const tl = gsap.timeline();

  for (let i = 0; i < chart.length; i++) {
    tl.to(`.chart__element__${i}`, {height: `${chart[i] * 6}px`, duration: 0.3}, "<+=0.15")
  }
}
</script>

<style lang="scss">
.chart {
  display: flex;
  gap: 2px;
  align-items: flex-end;
  &__section {
    color: $black-600;
    padding: 85px 20px 1px;
    height: 100vh;
  }
  &__title {
    font-size: 82px;
    margin-bottom: 55px;
    max-width: 1275px;
  }
  &__description {
    font-size: 22px;
    opacity: 0;
    transition: opacity 3s;
    max-width: 600px;
  }
  &__container {
    width: 100%;
    text-align: center;
  }
  &__value {
    color: $red-500;
    margin-bottom: 10px;
  }
  &__element {
    height: 0;
    background-color: $red-500;
  }
}
</style>
