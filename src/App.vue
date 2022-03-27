<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view />
      loggedIn: {{ loggedIn }}
      {{ profile.statusMessage }}
    </v-main>
  </v-app>
</template>

<script>
import liff from "@line/liff";

export default {
  name: "App",

  data() {
    return {
      loggedIn: false,
      profile: {},
      displayName: "",
    };
  },
  mounted() {
    // 最初に必ず実行する
    liff
      .init({ liffId: "1657007369-vbDgddDN" }) // LIFF IDを貼る
      .then(() => {
        this.loggedIn = liff.isLoggedIn();
        this.getProfile();
      })
      .catch((err) => {
        // Error happens during initialization
        this.occoredError = "error:" + err;
      })
      .sendMessages([
        {
          type: "image",
          originalContentUrl: require("./assets/logo.png"),
          previewImageUrl: "./assets/logo.png",
        },
      ]);
  },
  methods: {
    // ログインユーザーのプロフィールを取得する
    getProfile() {
      liff.getProfile().then((profile) => {
        this.profile = profile;
      });
    },
  },
};
</script>
