<template>
  <v-alert
    closable
    :text="text"
    :color="color"
    :icon="icon"
    :variant="variant"
  ></v-alert>
</template>

<script lang="ts" setup>
import { ALERT_VARIANT } from "@/constants/AlertVariant";
import type { AlertVariant } from "@/types/AlertVariant";
defineProps({
  text: {
    type: String,
    required: true,
  },
  color: {
    type: String,
    required: false,
    default: undefined,
  },
  icon: {
    type: String,
    required: false,
    default: "$info",
  },
  closable: {
    type: Boolean,
    required: false,
    default: true,
  },
  variant: {
    type: String as PropType<AlertVariant>,
    required: false,
    default: ALERT_VARIANT.OUTLINED,
  },
});
</script>
