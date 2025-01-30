<template>
  <div :class="computedClass">
    <label for="lang-switcher" class="sr-only"> Select language </label>
    <Select id="lang-switcher" size="sm" bare :variant="type">
      <option
          v-for="lang in $languages"
          :key="lang.text"
          :selected="lang.active"
      >
        {{ lang.text }}
      </option>
    </Select>
  </div>
</template>

<script setup lang="ts">
import Select from '../components/Select.vue'
import { computed } from 'vue'

defineProps({
  type: {
    type: String,
    default: () => 'negative',
  },
})

const computedClass = computed(() => {
  const base = 'language-switcher'
  return base
})
</script>
