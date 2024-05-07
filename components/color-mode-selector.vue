<template>
  <div class="flex space-x-2 items-center">
    <div class="text-gray-500 text-xs" v-if="showNextModeLabel">Change to {{ nextMode }}</div>
    <button @click="toggleMode"
            @mouseenter="showNextModeLabel = true"
            @mouseleave="showNextModeLabel = false"
            class="hover:bg-gray-200 dark:hover:bg-gray-600 px-2 py-1 text-gray-500 text-4xl md:text-base">{{ nextModeIcon }}</button>
  </div>
</template>

<script setup lang="ts">
const colorMode = useColorMode();
const showNextModeLabel = ref(false);

const modes = ['system', 'light', 'dark'];
const nextModeIcons = {
  system: '🌓',
  light: '🌕',
  dark: '🌑'
}

const nextMode = computed(() => {
const modeIndex = nextModeIndex(modes.indexOf(colorMode.preference), modes.length);
return modes[modeIndex];
  function nextModeIndex(currentModeIndex: Number, modesLength: Number) {
    const nextModeIndex = currentModeIndex + 1;
    return nextModeIndex === modesLength ? 0 : nextModeIndex;
  }
});
const nextModeIcon = computed(() => nextModeIcons[nextMode.value]);
const toggleMode = () => colorMode.preference = nextMode.value;
</script>