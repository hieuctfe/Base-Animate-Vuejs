<template>
  <canvas :width="size.width" :height="size.height" ref="canvas-ref" />
</template>

<script>
export default {
  name: "Canvas",
  props: {
    particles: {
      type: Array,
      default: () => [],
    },
    size: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {};
  },
  computed: {
    canvas() {
      return this.$refs["canvas-ref"];
    },
    ctx() {
      return this.canvas.getContext("2d", { alpha: false });
    },
  },
  methods: {
    start() {
      this.render();
    },
    render() {
      const ctx = this.ctx;
      ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
      this.particles.forEach((particle) => {
        particle.draw(ctx);
      });
      requestAnimationFrame(this.render);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
