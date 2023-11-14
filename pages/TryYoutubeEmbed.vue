<template>
  <main>
    <div v-for="(video, index) in youtubeId" :key="index" class="video">
      <Youtube :id="video.id"></Youtube>
    </div>
  </main>
</template>

<script>
import Youtube from './Youtube.vue'
export default {
  name: 'YoutubeEmbedTrial',
  components: { Youtube },
  data() {
    return {
      youtubeId: [
        {
          id: 'UPx4_tT9vT0',
        },
      ],
    }
  },
}
</script>
<style lang="scss" scoped></style>
