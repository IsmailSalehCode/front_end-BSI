<template>
  <v-menu theme="dark" v-if="!isMobile">
    <template v-slot:activator="{ props }">
      <v-btn
        size="small"
        rounded="0"
        v-bind="props"
        @click="expanded = !expanded"
        :append-icon="expanded == true ? 'mdi-chevron-up' : 'mdi-chevron-down'"
        >Projects</v-btn
      >
    </template>
    <v-list density="compact" class="pb-2">
      <v-list-item
        link
        v-for="project in projects"
        :key="project.id"
        :to="project.path"
        :class="isLast(project.id) ? '' : 'bottom-border'"
      >
        <v-list-item-title>{{ project.title }}</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-menu>
  <!-- ===================================================================== -->
  <v-list v-if="isMobile" density="compact">
    <v-list-group value="Projects">
      <template v-slot:activator="{ props }">
        <v-list-item v-bind="props" title="Projects"></v-list-item>
      </template>
      <v-list-item
        link
        v-for="project in projects"
        :key="project.id"
        :to="project.path"
        class="bottom-border"
      >
        <v-list-item-title>{{ project.title }}</v-list-item-title>
      </v-list-item>
    </v-list-group>
  </v-list>
</template>
<script>
import { useProjectStore } from "@/stores/projectStore";
import { mapState } from "pinia";
export default {
  props: { isMobile: Boolean },
  data() {
    return {
      expanded: false,
    };
  },
  computed: {
    ...mapState(useProjectStore, ["projects"]),
  },
  methods: {
    isLast(id) {
      const lastProject = this.projects[this.projects.length - 1];
      if (id == lastProject.id) {
        return true;
      }
      return false;
    },
  },
};
</script>
<style scoped>
.bottom-border {
  border-bottom: white solid 1px;
}
</style>
