<template>
  <div class="container">
  </div>
</template>

<script>
export default {
  name: 'SlidingWindow',
  data() {
    return {
      currSlidePos: 0,
      numberOfCellsShown: 0,
      sequenceArr: []
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
    let numberOfItemsToBePrepended = Number.parseInt((this.numberOfCellsShown - this.windowSize) / 2);
    let seq = this.sequenceLength - numberOfItemsToBePrepended;
    for (let i = 0; i < this.windowSize + 4; i++) {
      this.sequenceArr.push(seq);
      seq = (seq + 1) % this.sequenceLength;
    }
  },
  methods: {
    slide(n) {
      // get the last value in the sequence array
      let lastValue = (this.sequenceArr[this.sequenceArr.length - 1] + 1) % this.sequenceLength;

      // start appending further sequences in the array
      for (let i = 0; i < n; i++) {
        this.sequenceArr.push(lastValue);
        lastValue = (lastValue + 1) % this.sequenceLength;
      }

      // move the pos forward by n steps to slide using css
      this.currSlidePos += n;
    },
  },
  watch: {
    // just a watcher to print the sequence array whenever modified
    sequenceArr: {
      handler() {
        console.log(this.sequenceArr)
      },
      deep: true,
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
