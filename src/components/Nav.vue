<template>
  <div class="mb-10">
    <v-app-bar app color="deep-purple accent-4" dark class="c">
      <div class="d-flex align-center pl-12">
        <v-icon>mdi-store-outline</v-icon>
        <h1>USERs</h1>
      </div>

      <v-spacer></v-spacer>

      <div v-if="isLoggin" class="">
        <v-btn href="/home" text>
          <span class="mr-2">Home</span>
          <v-icon>mdi-home-edit-outline</v-icon>
        </v-btn>
        <v-btn href="/manage" text>
          <span class="mr-2">Manage</span>
          <v-icon>mdi-folder-edit-outline</v-icon>
        </v-btn>
        <v-btn style="float: right" @click="logout" color="error">Logout</v-btn>
      </div>

      <div v-if="!isLoggin" class="">
        <v-btn href="/register" text @click="newItemMode()">
          <v-icon>mdi-account-circle-outline</v-icon>
          <span class="mr-2">Sign Up</span>
        </v-btn>
        <v-btn text href="/">
          <v-icon>mdi-account-circle-outline</v-icon>
          <span class="mr-2">Login</span>
        </v-btn>
      </div>
    </v-app-bar>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isLoggin: localStorage.getItem("isLoggin"),
    };
  },
  methods: {
    logout() {
      localStorage.clear();
      this.$router.go('/');
      // window.location.reload(true);
    },
  },
};
</script>

<style></style>
