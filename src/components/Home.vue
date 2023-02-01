<template>
  <v-container>
    <v-app id="home">
      <div class="pt-5 mt-5">
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
  data() {
    return {
      fname: "",
      email: "",
    };
  },
  created() {
    //user is not authorized
    if (localStorage.getItem("token") === null) {
      this.$router.push("/");
    }
  },
  mounted() {
    axios
      .get("http://localhost:3000/user", {
        headers: { token: localStorage.getItem("token") },
      })
      .then((res) => {
        this.fname = res.data.user.fname;
        this.email = res.data.user.email;
      });
  },
};
</script>
