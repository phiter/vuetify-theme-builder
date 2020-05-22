<template>
  <v-card>
    <v-card-title
      :style="{
        background: baseColor,
        color: `${getColorByBgColor(baseColor)} !important`
      }"
      class="d-flex justify-center align-center"
    >
      <span>
        {{ colorName }}
        <span class="ml-2 body-2">
          {{ baseColor }}
        </span>
      </span>
      <v-spacer />
      <v-menu
        offset-x
        right
        :close-on-content-click="false"
      >
        <template #activator="{ on, attrs }">
          <v-btn
            icon
            title="Select color"
            v-bind="attrs"
            v-on="on"
          >
            <v-icon :color="getColorByBgColor(baseColor)">
              mdi-eyedropper
            </v-icon>
          </v-btn>
        </template>
        <v-color-picker v-model="colorRef" />
      </v-menu>
    </v-card-title>
    <v-list class="py-0">
      <v-list-item
        v-for="(color, variation) in colors"
        :key="variation"
        :style="{
          background: color,
          color: `${getColorByBgColor(color)} !important`,
        }"
      >
        {{ colorName }}-{{ variation }}
        <v-spacer />
        <span style="font-family: monospace;">
          {{ color }}
        </span>
      </v-list-item>
    </v-list>
  </v-card>
</template>

<script>
import { genVariations } from 'vuetify/lib/services/theme/utils';
import { colorToInt } from 'vuetify/lib/util/colorUtils';

export default {
  props: {
    colorName: {
      type: String,
      required: true,
    },
    dark: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    colors() {
      const { colorName, baseColor } = this;
      const variations = genVariations(colorName, colorToInt(baseColor));
      delete variations.base;

      return variations;
    },
    baseColor() {
      const { colorName } = this;
      const theme = this.dark ? 'dark' : 'light';
      const baseColor = this.$vuetify.theme.themes[theme][colorName];

      return baseColor;
    },
    colorRef: {
      get() {
        return this.baseColor;
      },
      set(v) {
        const { colorName } = this;
        const theme = this.dark ? 'dark' : 'light';
        this.$vuetify.theme.themes[theme][colorName] = v;
      },
    },
  },
  methods: {
    getColorByBgColor(bgColor) {
      if (!bgColor) { return ''; }
      return (parseInt(bgColor.replace('#', ''), 16) > 0xffffff / 2) ? '#000' : '#fff';
    },
  },
};
</script>
