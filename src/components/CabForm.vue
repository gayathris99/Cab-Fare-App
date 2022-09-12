<template>
  <div class="q-ma-lg card-shadow br-20 q-pa-md">
    <div class="fw-500 fs-36">Let's start our journey</div>
    <!-- Pickup -->
    <q-input
      outlined
      rounded
      v-model="pickup"
      placeholder="Enter pickup location"
      class="q-my-sm"
    >
      <template v-slot:prepend>
        <q-icon name="location_searching" />
      </template>
    </q-input>
    <!-- Drop -->
    <q-input
      outlined
      rounded
      placeholder="Enter drop location"
      class="q-my-sm"
      v-model="drop"
    >
      <template v-slot:prepend>
        <q-icon name="location_on" />
      </template>
    </q-input>
    <div class="row align-center q-mt-md">
      <q-btn
        rounded
        class="q-mr-md"
        :class="
          scheduleNow
            ? 'bg-primary text-whitesmoke'
            : 'text-primary bg-whitesmoke'
        "
        @click="scheduleRideNow"
        >Now</q-btn
      >
      <q-btn
        rounded
        :class="
          scheduleLater
            ? 'bg-primary text-whitesmoke'
            : 'text-primary bg-whitesmoke'
        "
        @click="scheduleRideLater"
        >Later</q-btn
      >
    </div>
    <div v-if="scheduleLaterRideDetails" class="fw-600 q-mt-md fs-16">
      Your ride is scheduled on {{ scheduleLaterRideDetails.formattedDate }} at
      {{ scheduleLaterRideDetails.formattedTime }}
    </div>
  </div>
  <q-dialog v-model="openPopup" persistent>
    <schedule-later-form @getScheduledRideDetails="getScheduledRideDetails" />
  </q-dialog>
</template>

<script>
import ScheduleLaterForm from "./ScheduleLaterForm.vue";
export default {
  name: "CabForm",
  components: { ScheduleLaterForm },
  data() {
    return {
      pickup: null,
      drop: null,
      scheduleNow: true,
      scheduleLater: false,
      openPopup: false,
      scheduleLaterRideDetails: null,
    };
  },
  methods: {
    scheduleRideNow() {
      this.scheduleNow = true;
      this.scheduleLater = false;
      this.scheduleLaterRideDetails = null;
    },
    scheduleRideLater() {
      this.scheduleLater = true;
      this.scheduleNow = false;
      this.openPopup = true;
      this.scheduleLaterRideDetails = null;
    },
    getScheduledRideDetails(rideDetails) {
      if (Object.keys(rideDetails).length) {
        this.scheduleLater = true;
        this.scheduleLaterRideDetails = rideDetails;
      } else {
        this.scheduleNow = true;
        this.scheduleLater = false;
      }
    },
  },
};
</script>
<style lang="scss" scoped></style>
