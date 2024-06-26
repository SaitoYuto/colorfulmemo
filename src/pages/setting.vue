<template>
  <v-main>
    <v-container>
      <v-label :text="INTERFACE_LABEL.PAGES.SETTING" />
      <v-divider :color="colorAttr.DIVIDER" :thickness="3" />
      <v-container>
        <BaseSelect
          :label="INTERFACE_LABEL.SELECT.MODE"
          :bgColor="colorAttr.SELECT"
          :items="modes"
          v-model:select="selectedModePair"
        ></BaseSelect>
        <BaseSelect
          :label="INTERFACE_LABEL.SELECT.COLOR"
          :bgColor="colorAttr.SELECT"
          :items="colors"
          v-model:select="selectedColorPair"
        ></BaseSelect>
        <BaseSelect
          :label="INTERFACE_LABEL.SELECT.BUTTON"
          :bgColor="colorAttr.SELECT"
          :items="variants"
          v-model:select="selectedVariantPair"
        ></BaseSelect>
      </v-container>
      <v-container class="d-flex justify-space-evenly mx-16">
        <BaseButton
          :text="INTERFACE_LABEL.BUTTON.SAVE"
          :color="colorAttr.BUTTON"
          :variant="variant"
          @click="save"
        />
      </v-container>
    </v-container>
  </v-main>
</template>

<script lang="ts" setup>
import { INTERFACE_LABEL } from "@/constants/InterfaceLabel";
import { BUTTON_VARIANT_PAIR } from "@/constants/ButtonVariantPair";
import { THEME_MODE_PAIR } from "@/constants/ThemeModePair";
import { THEME_COLOR_PAIR } from "@/constants/ThemeColorPair";
import { setting } from "@/stores/setting";
import { ButtonVariant } from "@/types/ButtonVariant";
import { useTheme } from "vuetify";

const settingStore = setting();
const router = useRouter();
const theme = useTheme();
const modes = THEME_MODE_PAIR.getList();
const colors = THEME_COLOR_PAIR.getList();
const variants = BUTTON_VARIANT_PAIR.getList();
const selectedModePair = ref(
  THEME_MODE_PAIR.get(settingStore.getThemeModeCode)
);
const selectedColorPair = ref(
  THEME_COLOR_PAIR.get(settingStore.getThemeColorCode)
);
const selectedVariantPair = ref(
  BUTTON_VARIANT_PAIR.get(settingStore.getButtonVariantCode)
);
const colorAttr = ref(settingStore.getThemeColorConst);
const variant = ref(settingStore.getButtonVariantConst);

/**
 * save selected value to store
 */
function save(): void {
  if (selectedModePair.value != undefined) {
    settingStore.setThemeMode(selectedModePair.value.code);
  }
  if (selectedColorPair.value != undefined) {
    settingStore.setThemeColor(selectedColorPair.value.code);
  }
  if (selectedVariantPair.value != undefined) {
    settingStore.setButtonVariant(selectedVariantPair.value.code);
  }
  router.go(0);
}

/**
 * monitor theme mode choice and modify the button style
 */
watch(selectedModePair, (newVal) => {
  if (newVal?.text != undefined) {
    theme.global.name.value = newVal.text.toLowerCase();
  }
});
/**
 * monitor theme mode choice and modify the button style
 */
watch(selectedColorPair, (newVal) => {
  if (newVal?.code != undefined) {
    settingStore.setThemeColor(newVal.code);
    colorAttr.value = settingStore.getThemeColorConst;
  }
});

/**
 * monitor button variant choice and modify the button style
 */
watch(selectedVariantPair, (newVal, oldVal) => {
  if (newVal?.text != undefined) {
    variant.value = newVal?.text as ButtonVariant;
  } else {
    variant.value = oldVal?.text as ButtonVariant;
  }
});
</script>
