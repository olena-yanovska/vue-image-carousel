<template>
  <ImageCarousel :images="images"/>
</template>

<script>
import ImageCarousel from './components/ImageCarousel.vue'

export default {
  name: 'App',
  components: {
    ImageCarousel,
  },
  data() {
    return {
      images: [],
    };
  },
  async created() {
    const response = await fetch("https://picsum.photos/v2/list?limit=30");
    const data = await response.json();
    this.images = data.map((image) => {
      return {
        url: image.download_url,
        author: image.author,
      };
    });
  },
}
</script>

<style>
</style>
