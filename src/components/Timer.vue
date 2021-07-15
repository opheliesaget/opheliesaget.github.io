<template>
  <div>
    <div id="timer">
      <img
        class="imgTimer"
        src="../assets/clock-icon.png"
        alt="Horloge icone"
      />
      <div id="morning" class="blockTimer">
        <img
          class="imgTimer"
          src="../assets/fleche-gauche.png"
          alt="flech haute"
          @click="addMorning"
        />
        <p>{{ hourUp }} : {{ minuteUp }}</p>
        <img
          class="imgTimer"
          src="../assets/fleche-right.png"
          alt="flech haute"
          @click="takeoffMorning"
        />
      </div>
      <div id="night" class="blockTimer">
        <img
          class="imgTimer"
          src="../assets/fleche-gauche.png"
          alt="flech haute"
          @click="addNight"
        />
        <p>{{ hourDown }} : {{ minuteDown }}</p>
        <img
          class="imgTimer"
          src="../assets/fleche-right.png"
          alt="flech haute"
          @click="takeoffNight"
        />
      </div>
      <p id="set" @click="timerRequest">Set</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  props: {
    mode: String,
    hourUp: Number,
    minuteUp: Number,
    hourDown: Number,
    minuteDown: Number
  },
  methods: {
    addMorning() {
      this.$emit("add-up");
    },
    takeoffMorning() {
      this.$emit("takeoff-up");
    },
    addNight() {
      this.$emit("add-down");
    },
    takeoffNight() {
      this.$emit("takeoff-down");
    },
    timerRequest() {
      this.$emit("set");
    }
  }
};
</script>

<style scoped>
#timer {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
}
#timer .imgTimer {
  width: 30px;
  height: 30px;
}
.blockTimer {
  display: flex;
  flex-direction: column;
  align-items: center;
}
#set {
  width: 40px;
  text-align: center;
  background-color: rgb(54, 200, 214);
  color: white;
  padding: 2px;
  border-radius: 12px;
  margin-bottom: 10px;
}
#set:active {
  background-color: white;
  border: 1px solid rgb(54, 200, 214);
  color: rgb(54, 200, 214);
}
</style>
