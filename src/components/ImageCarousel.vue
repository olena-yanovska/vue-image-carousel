<template>
  <div class="carousel no-scroll" ref="carousel">
    <div class="slides" :style="{ transform: `translateX(${position}px)`}">
      <div v-for="(image, index) in images" :key="image.id" class="slide">
        <div class="image-container">
          <img 
            :src="image.url" 
            :alt="image.author" 
            class="image"
            @click="selectImage(image.id)"
          >
        </div>
      </div>
    </div>
    
  </div>

  <div class="controls">
    <button class="button" @click="prevSlide">Prev</button>
    <button class="button" @click="nextSlide">Next</button>
  </div>

  <div v-if="selectedImages.length > 0" class="selected-images">
    <div v-for="(image, index) in selectedImages" :key="index" class="selected-image">
      <img v-if="image && image.url" 
        :src="image.url" 
        :alt="image.author"  
      />
      <p v-else>No image available</p>
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
      selectedImages: [],
      selected: null,
      currentIndex: 0,
      imagesToShow: 2,
      windowWidth: window.innerWidth,
    }
  },
  mounted() {
    this.handleResize();
    this.selectImage(0);
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
    selectImage(id) {
      const image = this.images.find(img => img.id === id);

        this.selectedImages.push(image);
    },
    updateVisibleImages() {
      this.desktopVisibleImages = Math.floor(window.innerWidth / this.imageSize);
    }
  }
}
</script>

<style>

.button {
  width: 100px;
  display: inline-block;
  padding: 10px;
  cursor: pointer;

  background-color: aquamarine;

  transition: box-shadow 0.3s, border 0.3s;

  border: 1px solid #e2e6e9;
  border-radius: 8px;

}

.button:hover {
  border: 1px solid #b4bdc4;
  box-shadow: 0px 2px 15px rgba(0, 0, 0, 0.05);
  font-weight: 500;
}
  
.carousel {
  display: flex;
  flex-direction: column;
  width: 100%;
  margin-top: 100px;
}

.no-scroll {
  overflow: hidden;
}

.slides {
  display: flex;
  flex-direction: row;
  transition: transform 0.3s ease-in-out;
}

.image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 400px;
  padding: 10px;
}

@media (max-width: 640px) {
  .image-container {
  width: 100vw;
}
}

.image {
  width: 100%;
  height: auto;
  max-height: 300px;
  border-radius: 8px;
  cursor: pointer;

  transition: transform 0.3s;
}

.image:hover {
  transform: scale(1.03);
}

.controls {
  display: block;
  margin: 50px auto;
  width: 300px;
}

button {
  margin: 0 10px;
}

.selected-images {
  display: flex;
  flex-wrap: wrap;
}

.selected-image {
  width: 100px;
  margin-right: 10px;
  margin-bottom: 10px;
}

.selected-image img {
  width: 100%;
  height: auto;
}

.selected-image.selected {
  border: 2px solid blue;
}
</style>