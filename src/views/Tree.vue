<template>
  <v-app>
    <v-system-bar app>System Bar</v-system-bar>
    <v-navigation-drawer id="id-drawer" app width="300" flex permanent>
      <!-- 
      <div style="flex: 1">Tags</div> -->
      <v-expansion-panels multiple accordion v-model="expanded_id_panels">
        <v-expansion-panel>
          <v-expansion-panel-header>Taxonomy</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-treeview
              dense
              activatable
              :items="taxonomy"
              :open.sync="open_nodes"
              :active.sync="active_nodes"
              return-object
              open-all
            >
              <template v-slot:prepend="{ open }">
                <v-icon>
                  {{ open ? "mdi-folder-open" : "mdi-folder" }}
                </v-icon>
              </template>
            </v-treeview>
          </v-expansion-panel-content>
        </v-expansion-panel>
        <v-expansion-panel>
          <v-expansion-panel-header>Tags</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-btn-toggle v-if="false" />
            <v-treeview
              dense
              :items="tags"
              _:open.sync="open_tags"
              _:active.sync="active_tags"
              return-object
            >
              <template v-slot:prepend="{ item }">
                <v-icon>
                  {{
                    item.state == "rejected"
                      ? "mdi-tag-off"
                      : item.state == "selected"
                      ? "mdi-tag"
                      : "mdi-tag-outline"
                  }}
                </v-icon>
              </template>
              <template v-slot:append="{ item }">
                <v-btn-toggle v-model="item.state">
                  <v-btn icon small color="green" value="selected">
                    <v-icon>mdi-check</v-icon>
                  </v-btn>
                  <v-btn icon small color="red" value="rejected">
                    <v-icon>mdi-close</v-icon>
                  </v-btn>
                  <div class="v-btn"
                    icon
                    small
                    v-if="item.star"
                    value="__star"
                    v-on:click="addTag(item)"
                  >
                    <v-icon>mdi-asterisk</v-icon>
                  </v-btn>
                </v-btn-toggle>
              </template>
            </v-treeview>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-navigation-drawer>
    <v-app-bar app
      ><v-text-field
        label="Identification"
        :value="identification"
        single-line
        full-width
      ></v-text-field
    ></v-app-bar>
    <v-main>
      <v-col cols="3" fill-height> Object Pane </v-col>
    </v-main>
  </v-app>
</template>

<script>
import Vue from "vue";
import taxonomy from "../taxonomy.json";
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
    addTag(item) {
      var state = item.state;
      console.log(item.state);

      // Reset state to previous value
      Vue.nextTick(function () {
        item.state = state;
      });
    },
  },
  watch: {
    active_nodes: function () {
      this.updateView();
    },
  },
  data: () => ({
    expanded_id_panels: [0, 1],
    open_nodes: [],
    active_nodes: [],
    identification: "",
    taxonomy: taxonomy.children,
    tags: [
      { id: "badfocus", name: "badfocus" },
      {
        id: "like",
        name: "like",
        star: true,
      },
      {
        id: "stage",
        name: "stage",
        children: [
          { id: "stage:egg", name: ":egg" },
          {
            id: "stage:nauplius",
            name: ":nauplius",
            children: [{ id: "stage:nauplius:1", name: ":1" }],
          },
        ],
      },
    ],
  }),
};
</script>

<style>
/* Behavior of accordeon panels */
#id-drawer .v-navigation-drawer__content {
  display: flex;
  flex-direction: column;
}
#id-drawer .v-expansion-panels {
  flex-direction: column;
  justify-content: start;
  flex: 1;
}
#id-drawer .v-expansion-panel {
  flex: 0;
  display: flex;
  flex-direction: column;
}
#id-drawer .v-expansion-panel.v-expansion-panel--active {
  flex: 1;
}
#id-drawer .v-expansion-panel-content {
  flex: 1;
  position: relative;
}
#id-drawer .v-expansion-panel-content__wrap {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow: auto;
  padding-left: 0;
  padding-right: 0;
}

/* Make treeview scrollable*/
#id-drawer .v-treeview-node__label {
  overflow: initial;
  text-overflow: initial;
}

#id-drawer .v-treeview {
  width: max-content;
}
</style>