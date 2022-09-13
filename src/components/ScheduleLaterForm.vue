<template>
  <div class="bg-whitesmoke card-shadow br-30 q-pa-lg">
    <div class="fw-600 fs-28">Schedule your ride at a later time</div>
    <p>Fix the date and time and we will be at your place hassle free!</p>
    <p class="fw-500">
      Please note: You cannot schedule later rides for today - Need to book
      atleast one day in advance
    </p>
    <div class="q-mt-lg">
      <!-- Select Date -->
      <div class="row align-center">
        <q-btn-dropdown
          :label="scheduleDate ? 'Change Date' : 'Select Date'"
          icon="calendar_month"
          class="without_icon"
        >
          <q-date
            minimal
            v-model="scheduleDate"
            :options="getDateRange"
            @update:model-value="onSelectDate"
          />
        </q-btn-dropdown>
        <div v-if="scheduleDate" class="fw-600 fs-20 q-ml-md q-mt-xs">
          {{ formattedDate }}
        </div>
      </div>

      <!-- Select Time -->
      <div class="row align-center q-mt-md">
        <q-btn-dropdown
          :label="scheduleTime ? 'Change time' : 'Select time'"
          icon="schedule"
          class="without_icon"
        >
          <q-time
            minimal
            v-model="scheduleTime"
            :minute-options="minuteOptions"
            @update:model-value="onSelectTime"
          />
        </q-btn-dropdown>
        <div v-if="scheduleTime" class="fw-600 fs-20 q-ml-md q-mt-xs">
          {{ formattedTime }}
        </div>
      </div>
    </div>
    <div class="q-mt-lg column">
      <q-btn
        label="Schedule Ride"
        class="q-my-sm bg-primary text-whitesmoke q-py-sm"
        @click="scheduleRide"
        v-close-popup="closePopup"
      />
      <q-btn outline label="Cancel" v-close-popup @click="cancelRide" />
    </div>
  </div>
</template>

<script>
export default {
  name: "ScheduleLaterForm",
  data() {
    return {
      scheduleDate: null,
      scheduleTime: null,
      formattedDate: null,
      formattedTime: null,
      closePopup: false,
      minuteOptions: [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55],
    };
  },
  methods: {
    getDateRange(date) {
      const currentDate = new Date();
      return date > currentDate.toLocaleDateString("en-ZA");
    },
    onSelectDate(date) {
      const dateArray = new Date(date).toString().split(" ");
      this.formattedDate = `${dateArray[2]} ${dateArray[1]} ${dateArray[3]}, ${dateArray[0]}`;
    },
    onSelectTime(time) {
      const timeArray = time.toString().split(":");
      const timeString = timeArray[0] > 12 ? "P.M" : "A.M";
      if (timeArray[0] > 12) {
        timeArray[0] = timeArray[0] - 12;
      }
      if (timeArray[0] === "00") {
        timeArray[0] = 12;
      }
      this.formattedTime = `${timeArray[0]}:${timeArray[1]} ${timeString}`;
    },
    scheduleRide() {
      if (!this.scheduleDate && !this.scheduleTime) {
        this.$q.notify({
          message: "Please select date and time",
          color: "red",
          position: "top",
          icon: "warning",
        });
      } else if (!this.scheduleDate) {
        this.$q.notify({
          message: "Please select date",
          color: "red",
          position: "top",
          icon: "warning",
        });
      } else if (!this.scheduleTime) {
        this.$q.notify({
          message: "Please select time",
          color: "red",
          position: "top",
          icon: "warning",
        });
      }

      if (this.scheduleDate && this.scheduleTime) {
        this.closePopup = true;
        const scheduledRideDetails = {
          date: this.scheduleDate,
          time: this.scheduleTime,
          formattedDate: this.formattedDate,
          formattedTime: this.formattedTime,
        };
        this.$emit("getScheduledRideDetails", scheduledRideDetails);
      }
    },
    cancelRide() {
      this.$emit("getScheduledRideDetails", {});
    },
  },
};
</script>

<style lang="scss" scoped>
.without-icon {
  :deep(.q-btn-dropdown__arrow) {
    display: none;
  }
}
</style>
