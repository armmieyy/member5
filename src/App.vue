<template>
  <v-app>
    <Nav/>
    <router-view> </router-view>
  </v-app>
</template>

<script>
import Nav from "./components/Nav.vue";
export default {
  name: "App",
  components: {
    Nav,
  },

  data: () => ({
    //
  }),
  methods: {
    logout() {
      this.$store.commit("logout");
    },
  },
  mounted() {
    this.$store.commit("initializaStore");
  },
};
</script>
