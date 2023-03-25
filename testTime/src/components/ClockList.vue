<template>

  <div class="clock-list">
      <clock-item
          v-for="(clock, idx) in clockList"
          :key="clock.createdTime"
          :clock="clock"
          @clockStart="doClockStart(idx)"
          @clockPause="doClockPause(idx)"
          @clockStop="doClockStop(idx)"
      />

      <button @click="addTimer" class="clock-list__add-clock">+</button>
  </div>

</template>

<script setup>
  import { ref, onMounted } from 'vue';
  import ClockItem from '@/components/ClockItem.vue'

  const clockList = ref([]);

  const addTimer = () => {
    clockList.value.push({
      enabled: false,
      savedTime: 0,
      lastStartTime: 0,
      dirty: false,
    });
  }

  const doClockStart = (clockIndex) => {
    if(clockList.value[clockIndex].enabled) return;

    clockList.value[clockIndex].enabled = true;
    clockList.value[clockIndex].dirty = true;
    clockList.value[clockIndex].lastStartTime = Date.now();
  }

  const doClockPause = (clockIndex) => {
    if(!clockList.value[clockIndex].enabled) return

    clockList.value[clockIndex].savedTime += Date.now() - clockList.value[clockIndex].lastStartTime;
    clockList.value[clockIndex].enabled = false;
  }

  const doClockStop = (clockIndex) => {
    clockList.value[clockIndex] = {
      enabled: false,
      savedTime: 0,
      lastStartTime: 0,
      dirty: false,
    }
  }

</script>