<template>
  <div>
    <v-row>
      <v-icon class="icon">mdi-file-tree</v-icon>
      ./
      <div v-if="path">
        {{ path }}
      </div>
      <v-col cols="12">
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Repository</th>
                <th class="text-left">Type of document</th>
                <th class="text-left">Link to file</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="content in contents" :key="content.name">
                <td v-if="isDirectory(content.type)">
                  <v-icon class="icon">mdi-folder</v-icon>
                  <button
                    class="directory teal--text text-decoration-underline"
                    @click="openDirectory(content.path)"
                  >
                    {{ content.name }}
                  </button>
                </td>
                <td v-else>
                  <v-icon class="icon">mdi-file-outline</v-icon>
                  {{ content.name }}
                </td>
                <td>
                  {{ content.type }}
                </td>
                <td>
                  <a :href="content.html_url">link</a>
                </td>
              </tr>
              <div v-if="typeof previousPath == 'string'">
                <v-btn class="ma-2" outlined color="teal" @click="goBack">
                  Back
                </v-btn>
              </div>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-progress-circular
          indeterminate
          color="primary"
          v-if="loading"
        ></v-progress-circular>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import { api } from "~api";
export default {
  props: ["repo"],
  data: () => ({
    contents: [],
    loading: false,
    directoryContent: true,
    previousPath: null,
    path: null,
  }),
  methods: {
    async listContent() {
      this.loading = true;
      const contents = await api.listContent(
        this.repo.owner.login,
        this.repo.name
      );
      this.contents = this.contents.concat(contents);
      this.previousPath = null;
      this.path = null;
      this.loading = false;
    },
    async listFolderContent(path) {
      this.loading = true;
      const contents = await api.listFolderContent(
        this.repo.owner.login,
        this.repo.name,
        path
      );
      let newPreviousPathList = path.split("/");
      this.path = path;
      newPreviousPathList.pop();
      const newPreviousPath = newPreviousPathList.join("/");
      this.previousPath = newPreviousPath;
      this.contents = this.contents.concat(contents);
      this.loading = false;
    },
    isDirectory(type) {
      return type == "dir";
    },
    openDirectory(path) {
      this.contents = [];
      this.listFolderContent(path);
    },
    goBack() {
      if (this.previousPath == "") {
        this.contents = [];
        this.listContent();
      } else {
        this.contents = [];
        this.listFolderContent(this.previousPath);
      }
    },
  },
  watch: {
    repo() {
      this.contents = [];
      this.listContent();
    },
  },
};
</script>

<style scoped>
.icon {
  width: 40px;
}
</style>
