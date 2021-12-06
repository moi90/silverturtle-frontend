<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app width="300">
      <!--<v-row>
      <v-text-field
        label="Identification"
        :value="identification"
        single-line
        full-width
      ></v-text-field>
    </v-row>-->
      <v-col cols="3">
        <v-treeview
          dense
          activatable
          :items="tree"
          :open.sync="open_nodes"
          :active.sync="active_nodes"
          return-object
          fill-height
        >
          <template v-slot:prepend="{}">
            <v-icon> mdi-tag-outline </v-icon>
          </template>
        </v-treeview>
        <v-divider></v-divider>
        Tags
      </v-col>
    </v-navigation-drawer>
    <v-main>
      <v-col cols="3" fill-height> Object Pane </v-col>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "Tree",
  methods: {
    updateView() {
      if (!this.active_nodes.length) {
        this.identification = "";
        return;
      }

      const node = this.active_nodes[0];

      this.identification = node.name;
      if (this.open_nodes.indexOf(node) == -1) {
        this.open_nodes.push(node);
      }
    },
  },
  watch: {
    active_nodes: function () {
      this.updateView();
    },
  },
  data: () => ({
    open_nodes: [],
    active_nodes: [],
    identification: "",
    tree: [
      {
        id: 1,
        name: "Applications :",
        children: [
          { id: 2, name: "Calendar : app" },
          { id: 3, name: "Chrome : app" },
          { id: 4, name: "Webstorm : app" },
        ],
      },
      {
        id: 5,
        name: "Documents :",
        children: [
          {
            id: 6,
            name: "vuetify :",
            children: [
              {
                id: 7,
                name: "src :",
                children: [
                  { id: 8, name: "index : ts" },
                  { id: 9, name: "bootstrap : ts" },
                ],
              },
            ],
          },
          {
            id: 10,
            name: "material2 :",
            children: [
              {
                id: 11,
                name: "src :",
                children: [
                  { id: 12, name: "v-btn : ts" },
                  { id: 13, name: "v-card : ts" },
                  { id: 14, name: "v-window : ts" },
                ],
              },
            ],
          },
        ],
      },
      {
        id: 15,
        name: "Downloads :",
        children: [
          { id: 16, name: "October : pdf" },
          { id: 17, name: "November : pdf" },
          { id: 18, name: "Tutorial : html" },
        ],
      },
      {
        id: 19,
        name: "Videos :",
        children: [
          {
            id: 20,
            name: "Tutorials :",
            children: [
              { id: 21, name: "Basic layouts : mp4" },
              { id: 22, name: "Advanced techniques : mp4" },
              { id: 23, name: "All about app : dir" },
            ],
          },
          { id: 24, name: "Intro : mov" },
          { id: 25, name: "Conference introduction : avi" },
        ],
      },
    ],
  }),
};
</script>
