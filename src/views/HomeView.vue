<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HomeMenu />
  </div>
</template>

<script>
// @ is an alias to /src
import HomeMenu from "@/components/HomeMenu.vue";

export default {
  name: "HomeView",
  components: {
    HomeMenu,
  },
};
</script>
