<template>
  <v-container>
    <v-app id="home">
      <div class="pt-5 mt-5">
        <v-btn style="float: right" @click="logout" color="error">Logout</v-btn>
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
    if (localStorage.getItem('token') === null) {
      this.$router.push('/login');
    }
  },
  mounted() {
    axios.get('http://localhost:3000/user', { headers: { token: localStorage.getItem('token')}})
      .then(res => {
        this.name = res.data.user.name;
        this.email = res.data.user.email;
      })
  },
  methods: {
    logout() {
      localStorage.clear();
      this.$router.push('/login');
    }
  }
};
</script>
