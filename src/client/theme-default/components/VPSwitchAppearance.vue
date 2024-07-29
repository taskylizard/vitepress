<script setup lang="ts">
import { inject, ref, watchPostEffect } from "vue";

import VPIconMoon from "./icons/VPIconMoon.vue";
import VPIconSun from "./icons/VPIconSun.vue";
import { useData } from "../../app/data";

const { isDark, theme } = useData();

const toggleAppearance = inject("toggle-appearance", () => {
  isDark.value = !isDark.value;
});

const switchTitle = ref('')

watchPostEffect(() => {
  switchTitle.value = isDark.value
    ? theme.value.lightModeSwitchTitle || 'Switch to light theme'
    : theme.value.darkModeSwitchTitle || 'Switch to dark theme'
})
</script>

<template>
  <button type="button" role="switch" :title="switchTitle" class="VPSwitchAppearance" :aria-checked="isDark"
    @click="toggleAppearance">
    <ClientOnly>
      <Transition name="fade" mode="out-in">
        <VPIconSun v-if="!isDark" class="sun" />
        <VPIconMoon v-else class="moon" />
      </Transition>
    </ClientOnly>
  </button>
</template>

<style scoped>
.VPSwitchAppearance {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 36px;
  height: 36px;
  color: var(--vp-c-text-2);
  transition: color 0.5s;

  &:hover {
    color: var(--vp-c-text-1);
    transition: color 0.25s;
  }

  &> :deep(svg) {
    width: 20px;
    height: 20px;
    fill: currentColor;
  }
}
</style>
