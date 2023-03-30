<template>
  <div class="carousel">
    <div class="slides" :style="{ transform: 'translateX(' + translateValue + '%)' }">
      <div v-for="(image, index) in images" :key="index" class="slide">
        <img :src="image.url" :alt="image.author" class="image">
      </div>
    </div>
    <div class="controls">
      <button @click="prevSlide">Prev</button>
      <button @click="nextSlide">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageCarousel',
  props: {
    images: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      currentIndex: 0,
      translateValue: 0
    }
  },
  methods: {
    nextSlide() {
      if (this.currentIndex < this.images.length - 1) {
        this.currentIndex++;
        this.translateValue -= 100;
      }
    },
    prevSlide() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
        this.translateValue += 100;
      }
    }
  }
}
</script>

<style scoped>
.carousel {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 400px;
}

.slides {
  display: flex;
  flex-direction: row;
}

.slide {
  flex-shrink: 0;
  width: 100%;
}

.image {
  width: 500px;
}

.controls {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
}

button {
  margin: 0 10px;
}
</style>
