<template>
  <nav id="main-navigation" :class="mainNavigationClass" aria-label="Main">
    <ul>
      <MainNavigationItem
          v-for="(navItem, index) in $clientData.mainNavigation"
          :navItem="navItem"
          :context="context"
          :level="0"
          :index="index"
          :showActiveNavigation="showActiveNavigation"
      />
      <!-- more button -->
      <li v-if="context == 'desktop'" id="more-button">
        <a
          href=""
          @click.prevent
          role="button"
          class="navy__has-children desktop-menu__more"
        >
          <span>Mehr</span>
          <SvgIcon icon="MoreFilled" size="lg" />
        </a>
        <ul id="more-container" />
      </li>
    </ul>
  </nav>
</template>

<script setup lang="ts">
import MainNavigationItem from './MainNavigationItem.vue'
import SvgIcon from '../components/SvgIcon.vue'
import { computed } from 'vue'

const props = defineProps({
  context: {
    type: String,
    required: true,
    validator: (prop) => ['desktop', 'mobile'].includes(prop as string),
  },
  showActiveNavigation: {
    type: Boolean,
    default: true
  },
})

const mainNavigationClass = computed(() => {
  let base = `main-navigation `
  if (props.context) base += `main-navigation--${props.context} `
  return base
})
</script>
