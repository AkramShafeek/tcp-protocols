<template>
  <div>    
    <SendWindow :windowParams="windowParams" />
    <ReceiveWindow :windowParams="windowParams" />
    <button @click="sendPacket">send packet</button>
    <button @click="sendAck">send ack</button>
  </div>
</template>

<script>
import SendWindow from './SendWindow.vue';
import ReceiveWindow from './ReceiveWindow.vue';

export default {
  name: 'GoBackN',
  data() {
    return {
      sendWindow: [0, 1, 2, 3, 4, 5, 6],
      Sf: 0,
      Sn: 0,
      Ssize: 7,
      seqLength: 8,
      nextSeqNo: 0,
      receiveWindow: [0, 1, 2, 3, 4, 5, 6],
      Rn: 0,
    }
  },
  components: { SendWindow, ReceiveWindow },
  computed: {
    windowParams() {
      console.log("Sending new window params")
      return {
        window: this.sendWindow,
        Sf: this.Sf,
        Sn: this.Sn,
        Ssize: this.Ssize,
        seqLength: this.seqLength,
        nextSeqNo: this.nextSeqNo,
      }
    }
  },
  methods: {
    sendPacket() {
      // check if window is full
      if ((this.Sf + this.Ssize) % this.seqLength == this.Sn) {
        // toggle a message to say window is full
        return;
      }
      this.nextSeqNo = this.Sn;
      this.Sn = (this.Sn + 1) % this.seqLength;
    },
    sendAck() {
      // check if window is full
      // if (this.Sf == this.Sn) {
      //   // toggle a message to say window is full
      //   return;
      // }
      // this.Sf = (this.Sf + 1) % this.seqLength;
    }
  }
}
</script>

<style scoped></style>
