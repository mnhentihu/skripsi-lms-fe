<template>
  <div class="youtube">
    <lite-youtube :videoid="id" :playlabel="label" />
  </div>
</template>
<script>
export default {
  name: 'YoutubeCom',
  props: {
    id: String,
    label: String,
  },
  fetchOnServer: false,
}
</script>
