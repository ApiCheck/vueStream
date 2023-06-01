<template>
  <div>
    <ParticipantAudio
      v-for="participantId in participantIds"
      :key="`participant_audio_${participantId}`"
      :participantId="participantId"
    />
  </div>
</template>

<script>
import { useMeeting } from "@videosdk.live/react-sdk";
import ParticipantAudio from "./ParticipantAudio.vue";

export default {
  name: "ParticipantsAudioPlayer",
  components: {
    ParticipantAudio,
  },
  data() {
    return {
      participantIds: [],
    };
  },
  mounted() {
    const mMeeting = useMeeting();
    const participants = mMeeting.value?.participants;
    
    if (participants) {
      this.participantIds = Array.from(participants.keys());
    }
  },
};
</script>
