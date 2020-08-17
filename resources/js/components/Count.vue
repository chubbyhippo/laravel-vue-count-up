<template>
  <span v-text="count"></span>
</template>

<script>
export default {
  props: ["to"],
  data() {
    return { count: 0, interval: null };
  },
  computed: {
    increment() {
      return Math.ceil(this.to / 30);
    }
  },
  methods: {
    tick() {
      if (this.count + this.increment >= this.to) {
        this.count = this.to;
        return clearInterval(this.interval);
      }

      this.count += this.increment;
    }
  },
  mounted() {
    const toBeShown = this.$el;
    console.log(toBeShown);

    const options = {};

    const observer = new IntersectionObserver((entries, observer) => {
      entries.forEach(entry => {
        console.log(entry);
        if (entry.isIntersecting) {
          this.interval = setInterval(this.tick, 40);
          observer.unobserve(entry.target);
        }
      });
    }, options);

    observer.observe(toBeShown);
  }
};
</script>
