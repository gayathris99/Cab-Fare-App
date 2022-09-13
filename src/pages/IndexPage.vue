<template>
  <q-page>
    <div class="row form-container">
      <div class="col-md-4 col-sm-12">
        <cab-form />
      </div>
    </div>
    <div class="map-container">
      <tom-tom-map city="Jayanagar" />
    </div>
  </q-page>
</template>

<script>
import CabForm from "components/CabForm.vue";
import TomTomMap from "components/TomTomMap.vue";

export default {
  name: "IndexPage",
  components: { CabForm, TomTomMap },
  // components: { CabForm },
};
</script>

<style lang="scss" scoped>
.form-container {
  position: absolute;
  z-index: 2;
  width: 100%;
  height: 100%;
}
.map-container {
  z-index: 1;
}
</style>
