<template>
  <v-dialog>
    <template #activator="{ on, attrs }">
      <v-btn
        outlined
        v-bind="attrs"
        v-on="on"
      >
        <v-icon left>
          mdi-code-braces-box
        </v-icon>
        Export
      </v-btn>
    </template>
    <v-card>
      <div class="pa-5">
        <pre>{{ theme }}</pre>
      </div>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  computed: {
    theme() {
      const { light, dark } = this.$vuetify.theme.themes;
      return {
        light,
        dark,
      };
    },
  },
};
</script>

<style>

</style>
