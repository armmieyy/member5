<template>
  <v-container>
    <v-app id="home">
      <div class="pt-5 mt-5">
        <v-btn style="float: right;"  @click="logout" color="error">Logout</v-btn>
        <h1>HELLO {{ fname }}</h1>
        <h2>your email is: {{ email }}</h2>
      </div>
    </v-app>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",

  // created() {
  //   if (localStorage.getItem("token") === null) {
  //     this.$router.push("/");
  //   }
  // },
  // mounted() {
  //   axios
  //     .get("http://localhost:3000/user", {
  //       headers: { token: localStorage.getItem("token") },
  //     })
  //     .then((res) => {
  //       this.fname = res.data.user.fname;
  //       this.email = res.data.user.email;
  //     });
  // },
  methods: {
    logout() {
      localStorage.clear();
      this.$router.push("/");
    },
  },
};
</script>
