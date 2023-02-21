<template>
  <homepage-app></homepage-app>
</template>

<script>
import HomepageApp from './components/HomepageApp.vue';
export default {
  name: 'App',
  components: {
    HomepageApp
  }
}
</script>

<style lang="scss">
* {
  ::-webkit-scrollbar {
    width: 10px;
  }

  ::-webkit-scrollbar-track {
    box-shadow: inset 0 0 5px red;
    border-radius: 10px;
    transition: all 1s;

    &:hover {
      box-shadow: inset 0 0 5px rgb(58, 136, 238);
    }
  }

  ::-webkit-scrollbar-thumb {
    background-clip: content-box;
    background-color: rgb(58, 136, 238);
    border-radius: 10px;

    // transition: background-color 1.8s;
    &:hover {
      background-color: red;
    }
  }
}
</style>
