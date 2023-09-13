<template>
  <div class="container">
    <div class="slider-container" :style="{ width: `${sliderContainerWidth}px` }">
      <div class="slider" :style="{ transform: `translateX(-${translateDist}px)` }">
        <div v-for="(seq, index) in sequenceArr" class="sequence" :key="index"
          :class="{ 'in-window': isInsideWindow(index), 'in-transit': isInTransit(index) }">
          {{ seq }}
          <div class="seq-bg-color" :class="{ 'slide-in': isInsideWindow(index) }"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SlidingWindow',
  data() {
    return {
      currSlidePos: 0,
      numberOfCellsShown: 0,
      sequenceArr: [],
      sfPointer: 2,
      snPointer: 2,
    }
  },
  props: {
    sequenceLength: Number,
    windowSize: Number,
    slideByN: Number,
    Sf: Number,
    Sn: Number,
    Ssize: Number,
    numOfPacketsSent: Number,
  },
  mounted() {
    // here create a sequence array based on window size and seqLen
    this.numberOfCellsShown = this.windowSize + 4;
    let numberOfItemsToBePrepended = 2;
    let seq = this.sequenceLength - numberOfItemsToBePrepended;
    for (let i = 0; i < this.windowSize + 4; i++) {
      this.sequenceArr.push(seq);
      seq = (seq + 1) % this.sequenceLength;
    }
  },
  methods: {
    slideSf(n) {
      // get the last value in the sequence array
      let lastValue = (this.sequenceArr[this.sequenceArr.length - 1] + 1) % this.sequenceLength;

      // start appending further sequences in the array
      for (let i = 0; i < n; i++) {
        this.sequenceArr.push(lastValue);
        lastValue = (lastValue + 1) % this.sequenceLength;
      }

      // move the pos forward by n steps to slide using css
      this.currSlidePos += n;

      this.sfPointer += n;
    },
    isInsideWindow(index) {
      return index >= this.sfPointer && index < this.sfPointer + this.windowSize;
    },
    isInTransit() {
      // return index % 2 != 0;
      return false;
    },
    slideSn(n) {
      this.snPointer += n;
    }
  },
  watch: {
    // just a watcher to print the sequence array whenever modified
    sequenceArr: {
      handler() {
        console.log(this.sequenceArr)
      },
      deep: true,
    }
  },
  computed: {
    translateDist() {
      return (this.currSlidePos * 20 + this.currSlidePos * 5);
    },
    sliderContainerWidth() {
      return (this.numberOfCellsShown * 20) + (this.numberOfCellsShown - 1) * 5;
    }
  }
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
  transition: all 900ms ease;
}

.transition {
  transition: all 900ms ease;
}

.sequence {
  box-sizing: border-box;
  width: 20px;
  height: 30px;
  border-radius: 3px;
  background-color: rgb(255, 255, 255);
  border: 1px dashed black;
  transition: 500ms all ease;
  position: relative;
  overflow: hidden;
}

.seq-bg-color {    
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(100, 190, 241);
  transform: translateY(100%);
  transition: 300ms all ease;
}

.slide-in{
  transform: translateY(0%);
}

.in-window {
  border: none;
  /* background-color: rgb(167, 199, 236); */
}

.in-transit {
  border: none;
  background-color: rgb(62, 141, 231);
}
</style>
