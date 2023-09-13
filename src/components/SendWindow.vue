<template>
  <div class="container">
    <SlidingWindow :sequence-length="8" :window-size="7" :slide-by-n="slideVal" ref="sendWindow"/>
    <button @click="$refs.sendWindow.slide(2)">slide</button>
  </div>
</template>

<script>
import SlidingWindow from './SlidingWindow.vue';

export default {
  name: 'SendWindow',
  data() {
    return {
      sequenceNos: [0, 1, 2, 3, 4, 5],
      slideLength: 0,
      windowSize: 6,
      isTransitionOn: true,
      slideVal: 0
    };
  },
  props: {
    windowParams: Object
  },
  methods: {

  },
  components: { SlidingWindow }
}
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 5px;
  background-color: aliceblue;
}

.slider-container {
  width: 120px;
  display: flex;
  /* padding: 5px; */
  background-color: antiquewhite;
  overflow: hidden;
}

.slider {
  display: flex;
  gap: 5px;
  /* padding: 5px; */
  background-color: rgba(100, 148, 237, 0);
}

.transition {
  transition: all 900ms ease;
}

.sequence {
  width: 20px;
  height: 30px;
  border-radius: 3px;
  background-color: rgb(177, 143, 223);
}
</style>
