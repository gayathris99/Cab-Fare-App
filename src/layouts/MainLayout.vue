<template>
  <q-layout>
    <header-component />
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import HeaderComponent from "components/HeaderComponent";
export default {
  name: "MainLayout",
  components: {
    HeaderComponent,
  },
};
</script>
