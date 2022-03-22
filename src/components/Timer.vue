<template>
  <div class="pv-timer">
    <div class="pv-timer__element pv-timer__minute">
      {{ minutes >= 10 ? minutes : "0" + minutes }}
    </div>
    <div class="pv-timer__dots">:</div>
    <div class="pv-timer__element pv-timer__seconds">
      {{ seconds >= 10 ? seconds : "0" + seconds }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  props: {
    status: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      minutes: 0,
      seconds: 0,
    };
  },
  methods: {
    start(workMinutes, breakMinutes) {
      this.minutes = workMinutes;
      this.seconds = 0;
      this.interval = setInterval(() => {
        if (this.seconds === 0 && this.minutes === 0) {
          this.$emit("changeStatus");
          if (this.status === "break") {
            this.minutes = workMinutes;
            this.seconds = 0;
          } else {
            this.minutes = breakMinutes;
            this.seconds = 0;
          }
        } else if (this.seconds === 0) {
          this.seconds = 59;
          this.minutes -= 1;
        } else {
          this.seconds -= 1;
        }
      }, 1000);
    },
    stop() {
      clearInterval(this.interval);
    },
  },
  mounted() {},
};
</script>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Fredoka:wght@300;400&family=Rubik:wght@500&display=swap");
.pv-timer {
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-family: "Rubik", sans-serif;
  font-size: 100px;
  @media (min-width: 641px) {
    font-size: 130px;
  }
  @media (min-width: 1281px) {
    font-size: 160px;
  }
}
</style>
