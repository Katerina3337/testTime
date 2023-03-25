<template>
  <div class="clock-item" :class="{disabled: !clock.enabled}">
    <p class="clock-item__time">{{passedFormat}}</p>
    <div class="clock-item__controls">
      <button v-if="!clock.enabled" @click="$emit('clockStart')" class="clock-item__control clock-item__start"></button>
      <button v-if="clock.enabled" @click="$emit('clockPause')" class="clock-item__control clock-item__pause">||</button>
      <button @click="$emit('clockStop')" class="clock-item__control clock-item__stop"></button>
    </div>
  </div>
</template>

<script setup>
  import {computed, defineProps, onMounted, ref} from "vue";

  const props = defineProps(['clock']);
  const passedTime = ref(0);



  const passedFormat = computed(() => {

    let seconds = Math.floor(passedTime.value / 1000);
    let minutes = Math.floor(seconds / 60);
    let hours = Math.floor(minutes / 60);

    seconds = seconds % 60;
    minutes = minutes % 60;


    let result = seconds;
    if(minutes) result = `${minutes}:` + result
    if(hours) result = `${hours}:` + result

    return props.clock.dirty ? result : 0;
  })


  onMounted(() => {
    setInterval(() => {
      if(props.clock.enabled){
        passedTime.value = (Date.now() - props.clock.lastStartTime) + props.clock.savedTime
      }
    }, 16)
  })

</script>