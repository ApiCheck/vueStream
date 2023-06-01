<template>
  <div v-if="isMeetingJoined">
    <div
      :style="{
        display: 'flex',
        flexDirection: 'column',
        height: '100vh',
        backgroundColor: messages.length > 0 ? messages[messages.length - 1].message : '#fff',
      }"
    >
      <ParticipantsAudioPlayer />
      <div
        :class="`grid-container ${remoteSpeakers.length > 1 ? 'more-participant' : 'single-participant'}`"
        style="display: grid; flex: 1; align-items: center; justify-items: center;"
      >
        <ParticipantView v-for="participant in remoteSpeakers" :key="participant.id" :participantId="participant.id" />
      </div>
      <Notification />
    </div>
  </div>
  <div v-else>
    <div
      style="
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
      "
    >
      <div class="loader"></div>
    </div>
  </div>
</template>

<script>
import { Constants, useMeeting, usePubSub } from "@videosdk.live/react-sdk";

import  ParticipantsAudioPlayer from "./ParticipantsAudioPlayer.vue";
import ParticipantView from "./ParticipantView.vue";

export default {
  name: "MeetingContainer",
  components: {
    ParticipantsAudioPlayer,
    ParticipantView,
  },
  data() {
    return {
      messages: [],
    };
  },
  computed: {
    isMeetingJoined() {
      return useMeeting().isMeetingJoined;
    },
    participants() {
      return useMeeting().participants;
    },
    localParticipant() {
      return useMeeting().localParticipant;
    },
    remoteSpeakers() {
      return Array.from(this.participants.values()).filter((participant) => {
        return participant.mode === Constants.modes.CONFERENCE && !participant.local;
      });
    },
  },
  created() {
    const { messages } = usePubSub("CHANGE_BACKGROUND");
    this.messages = messages;
  },
};
</script>


