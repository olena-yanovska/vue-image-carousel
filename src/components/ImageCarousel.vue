<template>
  <div class="carousel no-scroll" ref="carousel">
    <div class="slides" :style="{ transform: `translateX(${position}px)`}">
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
      position: 0,
      imageSize: 400,
      currentIndex: 0,
      imagesToShow: 2,
      windowWidth: window.innerWidth,
    }
  },
  mounted() {
    this.handleResize();
    window.addEventListener('resize', this.handleResize.bind(this));
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
  },
  watch: {
    windowWidth() {
      this.handleResize();
    },
  },
  methods: {
    nextSlide() {
      this.currentIndex++;

      if (this.currentIndex >= this.images.length - 1) {
        this.currentIndex = 0;
      }

      this.position = (-this.currentIndex * this.imageSize);
    },
    prevSlide() {
      this.currentIndex--;

      if (this.currentIndex < 0) {
        this.currentIndex = this.images.length - 1;
      }

      this.position = (-this.currentIndex * this.imageSize);
    },
    handleTransitionEnd() {
      this.currentIndex = (this.currentIndex + 1) % this.images.length;
      this.position = -this.currentIndex * this.imageSize;
    },
    handleResize() {
      const width = window.innerWidth;

      if (width < 640) {
        this.imageSize = width;
      } else {
        this.imageSize = 400;
      }
      this.position = -this.currentIndex * this.imageSize;
    },
  }
}
</script>

<style>
  
.carousel {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 500px;
}

.no-scroll {
  overflow: hidden;
}

.slides {
  display: flex;
  flex-direction: row;
  gap: 50px;
  transition: transform 0.3s ease-in-out;
}

.image {
  width: auto;
  height: 300px;
}

@media (max-width: 640px) {
  .image {
    width: 100vw;
    height: auto;
  }
}

.controls {
  position: absolute;
  bottom: 100px;
  left: 50%;
  transform: translateX(-50%);
}

button {
  margin: 0 10px;
}
</style>
