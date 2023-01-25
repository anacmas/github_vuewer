<template>
  <v-container>
    <GithubRepo @reposelected="onRepoSelected" />
    <GithubRepositories :repo="repo" />
  </v-container>
</template>

<script>
import GithubRepo from "./GithubRepo";
import GithubRepositories from "./GithubRepositories";
export default {
  components: {
    GithubRepo,
    GithubRepositories,
  },
  data: () => ({
    repo: null,
  }),
  methods: {
    onRepoSelected(repo) {
      this.repo = repo;
    },
  },
};
</script>
