<template>
  <div v-if="meetingId && token && participantId">
    <MeetingProvider
      :config="{
        meetingId,
        micEnabled: false,
        webcamEnabled: false,
        name: 'recorder',
        participantId,
      }"
      :token="token"
      :joinWithoutUserInteraction="true"
    >
      <MeetingContainer />
    </MeetingProvider>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';
import { MeetingProvider } from '@videosdk.live/react-sdk';
import MeetingContainer from './components/MeetingContainer.vue';

export default {
  name: 'App',
  components: {
    MeetingProvider,
    MeetingContainer,
  },
  setup() {
    const meetingId = ref(null);
    const token = ref(null);
    const participantId = ref(null);

    onMounted(() => {
      const location = window.location;
      const urlParams = new URLSearchParams(location.search);

      const paramKeys = {
        meetingId: 'meetingId',
        token: 'token',
        participantId: 'participantId',
      };

      Object.keys(paramKeys).forEach((key) => {
        paramKeys[key] = urlParams.get(key)
          ? decodeURIComponent(urlParams.get(key))
          : null;
      });

      meetingId.value = paramKeys.meetingId;
      token.value = paramKeys.token;
      participantId.value = paramKeys.participantId;
    });

    const hasValidParams = computed(() => {
      return meetingId.value && token.value && participantId.value;
    });

    return {
      meetingId,
      token,
      participantId,
      hasValidParams,
    };
  },
};
</script>


