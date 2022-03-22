<template>
  <div class="pomodoro-body" :style="bodyStyle">
    <div class="pomodoro-color" :style="colorStyle"></div>
    <div class="pomodoro-contents">
      <div v-show="status === 'work' || status === 'break'">
        <div class="pomodoro-status">{{ status }}</div>
        <Timer ref="timer" @changeStatus="changeStatus()" :status="status" />
      </div>
      <div v-show="status === 'menu'" class="pomodoro-menu">
        <h1>Pomodoro app</h1>
        <h5>Made by Cunev Serghei</h5>
      </div>
      <div v-show="status === 'settings'">
        <Card>
          <div><Button @click="status = 'menu'">Back</Button></div>
          <div class="pv-settings">
            <div class="pv-setting-label">work</div>
            <input
              class="pv-setting-input"
              type="number"
              v-model="workMinutes"
            />
          </div>
          <div class="pv-settings">
            <div class="pv-setting-label">break</div>
            <input
              class="pv-setting-input"
              type="number"
              v-model="breakMinutes"
            />
          </div>
        </Card>
      </div>
      <div style="display: flex; justify-content: center">
        <Button
          v-if="status === 'menu'"
          style="margin: 0 10px"
          @click="startTimer()"
          >Start</Button
        >
        <Button
          @click="status = 'settings'"
          v-if="status === 'menu'"
          style="margin: 0 10px"
          >Settings</Button
        >
        <Button
          v-if="status === 'work' || status === 'break'"
          style="margin: 0 10px"
          @click="stopTimer()"
          >Stop</Button
        >
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import Card from "../src/components/Card.vue";
import Timer from "../src/components/Timer.vue";
import Button from "../src/components/Button.vue";

export default defineComponent({
  name: "App",
  components: {
    Timer,
    Card,
    Button,
  },
  data() {
    return {
      status: "menu",
      isMounted: false,
      workMinutes: 25,
      breakMinutes: 5,
    };
  },
  mounted() {
    this.isMounted = true;
  },
  methods: {
    changeStatus() {
      this.status = this.status === "work" ? "break" : "work";
    },
    startTimer() {
      this.status = "work";
      this.$refs.timer.start(this.workMinutes, this.breakMinutes);
    },
    stopTimer() {
      this.$refs.timer.stop();
      this.status = "menu";
    },
  },
  computed: {
    timerProgress() {
      if (this.isMounted && this.$refs.timer) {
        if (this.status === "work") {
          return (
            (this.workMinutes * 60 -
              (this.$refs.timer.minutes * 60 + this.$refs.timer.seconds)) /
            (this.workMinutes * 60)
          ).toFixed(2);
        } else {
          return (
            (this.breakMinutes * 60 -
              (this.$refs.timer.minutes * 60 + this.$refs.timer.seconds)) /
            (this.breakMinutes * 60)
          ).toFixed(2);
        }
      } else {
        return 0;
      }
    },
    bodyStyle() {
      let style = "";
      if (this.status === "work") {
        style += "background-color: #DB301F;";
      } else {
        style += "background-color: #20A77C;";
      }
      return style;
    },
    colorStyle() {
      let style = "";
      if (this.status === "break") {
        style += "background-color: #DB301F;";
      } else {
        style += "background-color: #20A77C;";
      }
      style += "width: " + this.timerProgress * 100 + "vw;";
      return style;
    },
  },
});
</script>

<style lang="scss">
.pomodoro-body {
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
  z-index: -2;
}
.pomodoro-color {
  position: absolute;
  left: 0;
  width: 30vw;
  height: 100vh;
  z-index: 0;
  transition: 0.5s all ease-in;
}
.pomodoro-contents {
  z-index: 1000;
}
.pomodoro-status {
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-family: "Rubik", sans-serif;
  font-size: 40px;
  text-transform: uppercase;
  @media (min-width: 641px) {
    font-size: 50px;
  }
  @media (min-width: 1281px) {
    font-size: 60px;
  }
}
.pomodoro-menu {
  color: white;
  font-family: "Rubik", sans-serif;
  text-transform: uppercase;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  h1 {
    font-size: 30px;
    margin-bottom: 0px;
    white-space: nowrap;
  }
  h5 {
    white-space: nowrap;
    font-size: 12px;
  }

  @media (min-width: 641px) {
    h1 {
      font-size: 60px;
    }
    h5 {
      font-size: 18px;
    }
  }
  @media (min-width: 1281px) {
    h1 {
      font-size: 80px;
    }
    h5 {
      font-size: 25px;
    }
  }
}

.pv-settings {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  /* Chrome, Safari, Edge, Opera */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  /* Firefox */
  input[type="number"] {
    -moz-appearance: textfield;
  }
}
.pv-setting-label {
  font-size: 12px;
  margin-right: 10px;
  font-family: "Rubik", sans-serif;
  text-transform: uppercase;
  padding: 12px;
  @media (min-width: 641px) {
    font-size: 16px;
    margin-right: 10px;
    padding: 16px;
  }
}
.pv-setting-input {
  width: 50px;
  text-align: center;
  box-shadow: none;
  border: 1px solid gray;
  font-family: "Rubik", sans-serif;
  padding: 8px 16px;
  border-radius: 8px;
}
</style>
