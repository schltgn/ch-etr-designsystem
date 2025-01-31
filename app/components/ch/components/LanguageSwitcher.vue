<template>
  <div :class="computedClass">
    <label for="lang-switcher" class="sr-only"> Select language </label>
    <Select id="lang-switcher" size="sm" bare :variant="type" @change="changeLanguage">
      <option
          v-for="lang in $clientData.languages"
          :key="lang.text"
          :value="lang.url"
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

const changeLanguage = (url) => {
  if (url) {
    window.location.href = url;
  }
}
</script>
