<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      Vuetify theme builder
      <v-spacer />
      <export-dialog />
    </v-app-bar>

    <v-content>
      <v-container>
        <v-theme-provider
          v-for="theme in themes"
          :key="theme"
          :light="theme === 'light'"
          :dark="theme === 'dark'"
        >
          <v-card class="pa-5 mb-10" :dark="theme === 'dark'">
            <v-card-title>
              {{ theme }}
            </v-card-title>
            <v-row>
              <v-col
                v-for="color in colors"
                :key="color"
                cols="6"
                md="4"
                lg="3"
              >
                <ColorShades
                  :colorName="color"
                  :dark="theme === 'dark'"
                />
              </v-col>
            </v-row>
          </v-card>
        </v-theme-provider>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import ColorShades from './components/ColorShades.vue';
import ExportDialog from './components/ExportDialog.vue';

const themes = [
  'light',
  'dark',
];
const colors = [
  'primary',
  'secondary',
  'success',
  'error',
  'info',
  'warning',
];

export default {
  name: 'App',

  components: {
    ColorShades,
    ExportDialog,
  },

  data: () => ({
    colors,
    themes,
  }),
  created() {
    const hash = decodeURIComponent(window.location.hash);
    const json = hash.substr(1, hash.length);
    if (json) {
      const parsedColors = JSON.parse(json);

      Object.assign(this.$vuetify.theme.themes, parsedColors);
    }
  },
  watch: {
    '$vuetify.theme.themes': {
      deep: true,
      handler(value) {
        const str = JSON.stringify(value);
        window.location.replace(`#${str}`);
      },
    },
  },
};
</script>

<style>
  [data-app] {
    background: #ddd !important;
  }
</style>
