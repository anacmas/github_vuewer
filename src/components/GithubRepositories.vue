<template>
  <div>
    <v-row>
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
                <td
                  :class="
                    content.type == 'dir'
                      ? 'teal--text text-decoration-underline'
                      : ''
                  "
                >
                  {{ content.name }}
                </td>
                <td>
                  {{ content.type }}
                </td>
                <td>
                  <a :href="content.html_url">{{ content.type }}</a>
                </td>
              </tr>
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
  }),
  methods: {
    async listContent() {
      this.loading = true;
      const contents = await api.listContent(
        this.repo.owner.login,
        this.repo.name
      );
      this.contents = this.contents.concat(contents);
      this.loading = false;
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
