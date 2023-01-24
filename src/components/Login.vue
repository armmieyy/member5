<template>
  <v-container>
    <v-app id="app">
      <v-content>
        <v-container fluid fill-height>
          <v-layout align-center justify-center>
            <v-flex xs12 sm8 md4>
              <v-card class="elevation-12">
                <v-toolbar dark color="primary">
                  <v-toolbar-title>Login</v-toolbar-title>
                </v-toolbar>
                <v-card-text>
                  <form @submit.prevent="handleSubmit" ref="form">
                    <v-text-field
                      v-model="email"
                      prepend-icon="mdi-account"
                      name="email"
                      :rules="[rules.required, rules.email]"
                      label="Email"
                      type="email"
                    ></v-text-field>
                    <v-text-field
                      v-model="password"
                      prepend-icon="mdi-lock"
                      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                      :rules="[rules.required, rules.password]"
                      :type="show1 ? 'text' : 'password'"
                      counter
                      @click:append="show1 = !show1"
                      name="password"
                      label="Password"
                    ></v-text-field>

                    <v-card-actions class="mt-5">
                      <!-- <v-btn depressed to="/">Back</v-btn> -->
                      <v-spacer></v-spacer>
                      <v-btn @click="handleSubmit" color="success">Login</v-btn>
                    </v-card-actions>
                  </form>
                </v-card-text>
              </v-card>
            </v-flex>
          </v-layout>
        </v-container>
      </v-content>
    </v-app>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "Login",
  props: {
    source: String,
  },
  data() {
    return {
      email: "",
      show1: false,
      password: "",
      rules: {
        required: (value) => !!value || "Required.",
        min: (v) => v.length >= 8 || "Min 8 characters",
        email: (value) => {
          if (value.length > 0) {
            const pattern =
              /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
            return pattern.test(value) || "Invalid e-mail.";
          }
        },
        password: [
          (value) => !!value || "Required password.",
          (value) => (value && value.length >= 8) || "minimum 8 characters",
        ],
      },
    };
  },
  methods: {
    // async handleSubmit() {
    //   await axios.post("http://localhost:3000/auth/login", {
    //     email: this.email,
    //     password: this.password,
    //   });
      

    //   localStorage.setItem('token, respone.data.token')
    //   this.$store.dispatch('user', response.data.token)
    //   this.$router.push('/')
    // },
    async handleSubmit() {
      const response = await axios.post('http://localhost:3000/auth/login', {
        email: this.email,
        password: this.password
      });

      console.log(response)
    }
  },
};
</script>
<style>
.btn1 {
  margin-left: 260px;
}
</style>
