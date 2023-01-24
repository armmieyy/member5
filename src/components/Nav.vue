<template>
    <div class="mb-10">
  <v-app-bar app color="deep-purple accent-4" dark class="c">
    <div class="d-flex align-center pl-12">
      <v-icon>mdi-store-outline</v-icon>
      <h1>USERs</h1>
      <!-- <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        /> -->
    </div>

    <v-spacer></v-spacer>

    <!-- <v-btn href="/" text>
        <span class="mr-2">Home</span>
        <v-icon>mdi-home-outline</v-icon>
      </v-btn>
      <v-btn href="/order" text>
        <span class="mr-2">Order</span>
        <v-icon>mdi-archive-edit-outline</v-icon>
      </v-btn>
      <v-btn target="_blank" text @click="CartMode()">
        <span class="mr-2">Cart</span>
        <v-icon>mdi-cart-outline</v-icon>
      </v-btn> -->
    <v-btn href="/manage" text>
      <span class="mr-2">Manage</span>
      <v-icon>mdi-folder-edit-outline</v-icon>
    </v-btn>
    <v-btn href="/register" text @click="newItemMode()">
      <v-icon>mdi-account-circle-outline</v-icon>
      <span class="mr-2">Sign Up</span>
    </v-btn>
    <v-btn text href="/">
      <v-icon>mdi-account-circle-outline</v-icon>
      <span class="mr-2">Login</span>
    </v-btn>
    <!-- <v-btn target="_blank" text @click="LoginMode()" v-if="!auth">
        <span class="mr-2">Login</span>
        <v-icon>mdi-account-circle-outline</v-icon>
      </v-btn>
      <v-btn target="_blank" text @click="LoginMode()" v-else>
        <span class="mr-2">{{ username }}</span>
        <v-icon>mdi-account-circle-outline</v-icon>
      </v-btn> -->
    <!-- <v-btn class="ml-16" color="success" @click="newItemMode()"
      >+ Create New Product</v-btn
    > -->
  </v-app-bar>
</div>
</template>

<script>
export default {};
</script>

<style></style>
