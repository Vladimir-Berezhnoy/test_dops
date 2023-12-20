<template>
  <HomeComponent />
</template>

<script>
// @ is an alias to /src
import HomeComponent from "@/components/HomeComponent.vue";

export default {
  name: "HomeView",
  components: {
    HomeComponent,
  },
};
</script>

<style lang="scss">
.test {
  position: absolute;
  left: -99999px;
}
</style>
