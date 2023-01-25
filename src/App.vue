<template>
  <v-app>
    <v-app-bar app color="#00838F" dark>
      <div class="d-flex align-center">
        <h1>Github Finder</h1>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/anacmas/github_vuewer"
        target="_blank"
        text
        ><v-icon>mdi-github</v-icon>
      </v-btn>

      <v-btn
        href="https://www.linkedin.com/in/ana-carolina-milos-17259787/"
        target="_blank"
        text
        ><v-icon>mdi-linkedin</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <GithubVuewer />
    </v-main>
  </v-app>
</template>

<script>
import GithubVuewer from "./components/GithubVuewer";

export default {
  name: "App",

  components: {
    GithubVuewer,
  },

  data: () => ({
    //
  }),
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Sofia+Sans&family=Ubuntu:wght@500&display=swap");

h1 {
  font-family: "Ubuntu", sans-serif;
}
</style>
