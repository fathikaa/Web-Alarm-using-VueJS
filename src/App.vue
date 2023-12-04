<template>
  <h1>{{ jam }}:{{ menit }}:{{ detik }}</h1>
  <input type="text" v-model="alarm_jam" />
  <input type="text" v-model="alarm_menit" />
  <button @click="addAlarm()">Add Alarm</button>
  <button @click="stopAlarm()" v-if="alarmshowstop">Stop Alarm</button>
  <button @click="snoozeAlarm(3000)" v-if="alarmshowstop">
    Snooze Alarm (3 s)
  </button>
  <button @click="snoozeAlarm(5000)" v-if="alarmshowstop">
    Snooze Alarm (5 s)
  </button>
  <button @click="snoozeAlarm(10000)" v-if="alarmshowstop">
    Snooze Alarm (10 s)
  </button>
  <ul>
    <li v-for="alarm in alarms" v-bind:key="alarm.jam">
      <p>
        {{ alarm.jam }}:{{ alarm.menit }}
        <button @click="toogleAlarm(alarm)">{{ alarm.enabled }}</button>
        <button @click="deleteAlarm(alarm)">Delete</button>
      </p>
    </li>
  </ul>
</template>

<script>
var audio = new Audio("horse.mp3");
export default {
  name: "App",
  data() {
    return {
      jam: 0,
      menit: 0,
      detik: 0,
      alarms: [],
      alarm_jam: 0,
      alarm_menit: 0,
      alarmshowstop: false,
    };
  },
  methods: {
    updateClock() {
      const d = new Date();
      this.jam = d.getHours();
      this.menit = d.getMinutes();
      this.detik = d.getSeconds();

      this.alarms.forEach(this.checkAlarm);
    },
    checkAlarm(item) {
      if (
        item.enabled &&
        item.jam == this.jam &&
        item.menit == this.menit &&
        this.detik == 0
      ) {
        this.soundAlarm();
      }
    },
    addAlarm() {
      this.alarms.push({
        jam: this.alarm_jam,
        menit: this.alarm_menit,
        enabled: true,
      });
    },
    toogleAlarm(alarm) {
      alarm.enabled = !alarm.enabled;
    },
    deleteAlarm(alarm) {
      const index = this.alarms.indexOf(alarm);
      if (index > -1) {
        this.alarms.splice(index, 1);
      }
    },
    soundAlarm() {
      this.alarmshowstop = true;
      audio.lopp = true;
      audio.play();
    },
    stopAlarm() {
      this.alarmshowstop = false;
      audio.loop = false;
      audio.pause();
    },
    snoozeAlarm(3 s) {
      this.stopAlarm();
      setTimeout(this.soundAlarm, 3000);
    },
    snoozeAlarm() {
      this.stopAlarm();
      setTimeout(this.soundAlarm, 5000);
    },
    snoozeAlarm() {
      this.stopAlarm();
      setTimeout(this.soundAlarm, 10000);
    },
  },
  mounted() {
    this.updateClock();
    setInterval(this.updateClock, 1000);
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
