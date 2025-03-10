<template>
  <div class="dark-mode-toggle" @click="toggleMode">
    <div class="toggle-container" :class="{ 'is-dark': isDark }">
      <q-icon :name="props.lightIcon" class="toggle-icon light-icon" />
      <q-icon :name="props.darkIcon" class="toggle-icon dark-icon" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, watch } from 'vue'
import { useQuasar } from 'quasar'
import { useDark } from '../composables/dark'
import { mdiMoonWaningCrescent, mdiWhiteBalanceSunny } from '@quasar/extras/mdi-v7'

const props = defineProps({
  darkIcon: {
    type: String,
    default: mdiMoonWaningCrescent, // Default dark mode icon
  },
  lightIcon: {
    type: String,
    default: mdiWhiteBalanceSunny, // Default light mode icon
  },
})

const emit = defineEmits(['update:mode'])

const $q = useQuasar()
const { toggleDark } = useDark()

const isDark = computed(() => $q.dark.isActive)

const toggleMode = () => {
  toggleDark()
  emit('update:mode', isDark.value ? 'dark' : 'light')
}

// Watch for changes in Quasar dark mode
watch(
  () => $q.dark.isActive,
  (newVal) => {
    emit('update:mode', newVal ? 'dark' : 'light')
  },
)
</script>

<style scoped lang="scss">
.dark-mode-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  width: 60px;
  height: 30px;
  background-color: $brand-light-bg;
  border: $brand-primary solid 1px;
  border-radius: 15px;
  position: relative;
  overflow: hidden;
}

.toggle-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 100%;
  position: relative;
  transition: transform 0.3s ease;
}

.toggle-container.is-dark .light-icon {
  transform: translateX(-100%);
}

.toggle-container.is-dark .dark-icon {
  transform: translateX(0);
}

.toggle-icon {
  color: $brand-primary;
  font-size: 20px;
  width: 50%;
  text-align: center;
  position: absolute;
  transition: transform 0.3s ease;
}

.light-icon {
  left: 0;
  transform: translateX(0);
}

.dark-icon {
  right: 0;
  transform: translateX(100%);
}

body.body--dark {
  .dark-mode-toggle {
    background-color: $brand-dark-bg;
  }
}
</style>
