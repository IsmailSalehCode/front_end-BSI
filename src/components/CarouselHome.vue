<template>
  <v-card :max-width="cardMaxWidth" class="mx-auto" rounded="0" flat>
    <v-carousel
      :height="cardMaxWidth / 1.5"
      cycle
      show-arrows="hover"
      hide-delimiters
    >
      <v-carousel-item
        v-for="project in carouselProjects"
        :key="project.id"
        eager
      >
        <v-img
          class="cardTitle"
          :src="project.landing.photo"
          height="100%"
          width="100%"
          eager
          style="background-color: #262626"
          cover
        >
          <div v-if="mdAndUp">
            <v-card-text class="titleBg py-0">
              {{ project.title }}
            </v-card-text>
            <v-card-title class="titleBg pt-0">
              {{ project.address }}
            </v-card-title>
          </div>
          <v-card-text
            class="titleBg py-0 px-1"
            justify="center"
            v-if="smAndDown"
          >
            <v-icon x-small dark :icon="iconMapMarker"></v-icon>
            <span style="font-size: 0.85em"
              ><i> {{ project.title }}, {{ project.address }}</i></span
            >
          </v-card-text>
          <template v-slot:placeholder>
            <div class="d-flex align-center justify-center fill-height">
              <v-progress-circular
                color="grey-lighten-4"
                indeterminate
              ></v-progress-circular>
            </div>
          </template>
        </v-img>
      </v-carousel-item>
    </v-carousel>
  </v-card>
</template>

<script>
import { useDisplay } from "vuetify";
import { projects } from "@/stores/projectStore";
import { mdiMapMarker } from "@mdi/js";

export default {
  setup() {
    // Destructure only the keys we want to use
    const { smAndDown, mdAndUp, name } = useDisplay();
    const iconMapMarker = mdiMapMarker;
    return { smAndDown, mdAndUp, name, iconMapMarker };
  },

  computed: {
    carouselProjects() {
      // console.log(projects);
      let res = [];
      for (let i = 0; i < projects.length; i++) {
        const project = projects[i];
        if (project.landing.photo != null) {
          res.push(project);
        }
      }
      return res.sort((a, b) => a.landing.order - b.landing.order);
    },

    cardMaxWidth() {
      let width = "1000";

      switch (this.name) {
        case "xs":
          width = 624;
          break;
        case "sm":
          width = 700;
          break;
        case "md":
        case "lg":
          width = 900;
          break;
        case "xl":
          width = 1060;
          break;
        case "xxl":
          width = 1080;
          break;
      }
      return width;
    },
  },
};
</script>

<style>
.cardTitle {
  background-color: white;
  color: white;
  font-weight: 550;
  align-items: flex-end;
}
.titleBg {
  /* transition: all 0.2s ease; */
  background-color: rgba(0, 0, 0, 0.39);
  color: white !important;
}
</style>
