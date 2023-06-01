<template>
  <div class="participant-view" :style="participantViewStyle">
    <template v-if="webcamOn && webcamStream">
      <video ref="videoPlayer" playsinline controls :muted="true" autoplay :srcObject="videoStream"></video>
    </template>
    <template v-else>
      <div class="placeholder">
        {{ displayName.charAt(0).toUpperCase() }}
        <h3>Score card Here 0</h3>
      </div>
    </template>
    <div class="participant-info">
      {{ displayName }}
      <h3>Score card Here 1</h3>
      <template v-if="!micOn">
        <MicOffIcon fillcolor="#fff" height="18" width="18" />
      </template>
    </div>
  </div>
</template>

<script>
import { useParticipant } from "@videosdk.live/react-sdk";
import MicOffIcon from "./MicOffIcon.vue";

export default {
  name: "ParticipantView",
  components: {
    MicOffIcon,
  },
  props: {
    participantId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      webcamOn: false,
      webcamStream: null,
      displayName: "",
      micOn: false,
      videoStream: null,
      participantViewStyle: {
        width: "100%",
        height: "100vh",
        display: "flex",
        flex: 1,
        alignItems: "center",
        justifyContent: "center",
        position: "relative",
        backgroundColor: "#1A1C22",
        borderRadius: "10px",
        overflow: "hidden",
      },
    };
  },
  mounted() {
    const {
      webcamOn,
      webcamStream,
      displayName,
      micOn,
    } = useParticipant(this.participantId);

    this.webcamOn = webcamOn;
    this.webcamStream = webcamStream;
    this.displayName = displayName;
    this.micOn = micOn;

    if (webcamOn && webcamStream) {
      this.videoStream = new MediaStream();
      this.videoStream.addTrack(webcamStream.track);

      this.$refs.videoPlayer.srcObject = this.videoStream;
      this.$refs.videoPlayer.play().catch((err) => {
        console.log(err, "participant video error");
      });
    }
  },
};
</script>

<style scoped>
.placeholder {
  font-size: 50px;
  color: #fff;
}

.participant-info {
  position: absolute;
  left: 10px;
  bottom: 10px;
  background-color: #050A0E;
  color: #fff;
  padding: 4px;
  border-radius: 4px;
  align-items: center;
  justify-items: center;
  display: flex;
}

video {
  height: 100%;
  width: 100%;
}
</style>
