<template>
  <q-page class="row">
    <div class="col-md-4 col-sm-12">
      <cab-form />
    </div>
  </q-page>
</template>

<script>
import CabForm from "components/CabForm.vue";
export default {
  name: "IndexPage",
  components: { CabForm },
};
</script>
