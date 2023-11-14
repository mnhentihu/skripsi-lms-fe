<template>
  <main class="background-pages">
    <img src="~assets/img/bg.png" alt="" class="bgimage" />
  </main>
</template>

<script>
export default {
  name: "PageBackground"
}
</script>

<style lang="scss" scoped>
.bgimage {
    position: fixed;
    z-index: -1;
    object-fit: fill;
    width: 100%;
    height: 100%;
  }

</style>