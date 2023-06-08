<template>
  <v-app>
    <v-app-bar fixed flat app color="#2581ba">

      <v-btn class="mx-0" fab dark small color="purple" elevation="0">
        <img class="mx-auto" src="../assets/images/logo.png" width="45" />
      </v-btn>
      <div style="width: 10px"></div>
      <v-btn color="white" class="ml-2 white--text" elevation="0" text>
        <v-icon color="#fff">mdi-poll</v-icon>
        <span class="white--text ml-1">ລາຍຮັບ</span>
      </v-btn>
      <v-spacer />
      <div style="height:15px;border-left:1px solid white" class="mr-2">
      </div>
      <div class="d-flex align-center">
        <v-icon color="white">mdi-account-sync</v-icon>
        <span class="white--text">{{ userName }}</span>
      </div>

      <v-btn text color="white" rounded @click="onLogout">
        <v-icon dark>mdi-power </v-icon>
        <span class="white--text">ອອກລະບົບ</span>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      active: '',
      clipped: false,
      drawer: false,
      fixed: false,
      miniVariant: false,
      menu: false,
      userName: ''
    }
  },
  mounted() {
  },
  methods: {
    onLogout() {
      localStorage.clear();
      this.$router.push('/')
    },
  }
}
</script>