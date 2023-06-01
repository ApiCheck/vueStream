<template>
  <audio autoplay playsinline :controls="false" ref="audioPlayer" />
</template>

<script>
export default {
  name: 'ParticipantAudio',
  props: {
    participantId: String
  },
  mounted() {
    this.setupAudioPlayer();
  },
  beforeDestroy() {
    this.cleanupAudioPlayer();
  },
  methods: {
    setupAudioPlayer() {
      const { micOn, micStream, isLocal } = this.useParticipant(this.participantId);
      if (!isLocal && this.$refs.audioPlayer && micOn && micStream) {
        const mediaStream = new MediaStream();
        mediaStream.addTrack(micStream.track);
  
        this.$refs.audioPlayer.srcObject = mediaStream;
        this.$refs.audioPlayer.play().catch((err) => {});
      } else {
        this.$refs.audioPlayer.srcObject = null;
      }
    },
    cleanupAudioPlayer() {
      this.$refs.audioPlayer.srcObject = null;
    },
    useParticipant(participantId) {
      // Your implementation of the useParticipant function goes here
      // Make sure to return the micOn, micStream, and isLocal values
    }
  }
};
</script>

<style scoped>
/* Your component styles here */
</style>
