<template>
  <section class="result__chart__section section" id="result">
    <article class="result__chart">
      <div
        :class="`result__chart__item result__chart__item__${index}`"
        v-for="(item, index) in chart"
        :key="index"
      >
        <p class="result__chart__item__title">{{ item.title }}</p>
        <p class="result__chart__item__value">{{ item.value }}</p>
      </div>
    </article>
  </section>
</template>

<script setup>
import { gsap } from "gsap/all";
import { defineProps, watch } from "vue";

const props = defineProps({
  animation: {
    type: String,
  },
});

watch(() => props.animation, (newVal) => {
  if (newVal === 'result') {
    startChartAnimation();
  }
});

const chart = [
  { title: "Revenue generated for clients", value: 1570, width: 100 },
  { title: "Conversation opened", value: 1200, width: 55 },
  { title: "Leads generated via targeted", value: 378, width: 100 },
  { title: "Calls scheduled for clients", value: 197, width: 45 },
];

function startChartAnimation() {
  const tl = gsap.timeline();

  for (let i = 0; i < chart.length; i++) {
    tl.to(
      `.result__chart__item__${i}`,
      { width: `${chart[i].width}%`, duration: 0.4 },
      "<+=0.2"
    );
  }
}
</script>

<style lang="scss">
.result {
  &__chart {
    display: flex;
    flex-direction: column;
    gap: 1px;
    align-items: flex-end;
    &__section {
      width: 100%;
      color: $red-500;
      padding: 1px 20px;
      overflow: hidden;
    }
    &__item {
      width: 0;
      height: 92px;
      background-color: $black-600;
      display: flex;
      align-items: center;
      gap: 20px;
      color: $red-500;
      padding-left: 140px;
      padding-right: 40px;
      text-align: end;
      &:first-child {
        justify-content: flex-end;
      }
      &__title {
        font-size: 22px;
        max-width: 200px;
      }
      &__value {
        font-size: 62px;
      }
    }
  }
}
</style>
