<template>
  <div class="hello">
    <base-canvas
      v-if="isCompletedInit"
      ref="canvas"
      :particles="particles"
      :size="canvasSize"
    />
  </div>
</template>

<script>
import BaseCanvas from "./BaseCanvas";
import { Partical } from "../model/partical";
import Logo from "../assets/Line logo.png";
// import Logo from "../assets/logo.png";
import gsap from "gsap";
export default {
  name: "Animation",
  props: {
    msg: String,
  },
  components: {
    BaseCanvas,
  },
  data() {
    return {
      particles: [],
      isCompletedInit: false,
      canvasSize: {
        width: 500,
        height: 500,
      },
      canvasScale: 3,
    };
  },
  async mounted() {
    this.isCompletedInit = false;
    let image = await this.loadImage(Logo);
    this.initCanvas(image);
    this.isCompletedInit = true;
    this.$nextTick(() => {
      this.$refs["canvas"].start();
    });
  },
  methods: {
    initCanvas(image) {
      const canvas = document.createElement("canvas");
      this.canvasSize = {
        width: image.width * this.canvasScale,
        height: image.height * this.canvasScale,
      };
      const ctx = canvas.getContext("2d");
      ctx.drawImage(image, 0, 0);
      let data = ctx.getImageData(0, 0, image.width, image.height);
      for (let x = 0; x < data.width; x++) {
        for (let y = 0; y < data.height; y++) {
          const index = (x + y * data.width) * 4;
          const partical = new Partical({
            x: this.canvasSize.width / 2,
            y: this.canvasSize.height / 2,
            rgba: {
              r: data.data[index],
              g: data.data[index + 1],
              b: data.data[index + 2],
              a: data.data[index + 3],
            },
            width: 3,
            height: 3,
          });
          this.particles.push(partical);
          gsap.to(partical, Math.random() * 2, {
            x: x * this.canvasScale,
            y: y * this.canvasScale,
            delay: y / 20,
            ease: "Elastic.easeOut",
          });
        }
      }
    },
    loadImage(imagePath) {
      return new Promise((resolve, reject) => {
        const image = new Image();
        image.crossOrigin = "Anonymous";
        image.addEventListener("load", () => {
          resolve(image);
        });
        image.addEventListener("error", (err) => {
          reject(err);
        });
        image.src = imagePath;
      });
    },
  },
};
</script>

<style scoped></style>
