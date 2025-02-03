<template>
  <nav id="main-navigation" :class="mainNavigationClass" aria-label="Main">
    <ul>
      <MainNavigationItem
          v-for="(navItem, index) in visibleNavItems"
          :nav-item="navItem"
          :context="context"
          :level="0"
          :index="index"
          :show-active-navigation="showActiveNavigation"
          :parent-nav-item="null"
      />
      <!-- more button -->
      <li v-if="context === 'desktop'" id="more-button">
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

<script setup>
import MainNavigationItem from './MainNavigationItem.vue'
import SvgIcon from '../components/SvgIcon.vue'
import {computed, getCurrentInstance} from 'vue'

const props = defineProps({
  context: {
    type: String,
    required: true,
    validator: (prop) => ['desktop', 'mobile'].includes(prop),
  },
  showActiveNavigation: {
    type: Boolean,
    default: true
  },
});

const mainNavigationClass = computed(() => {
  let base = `main-navigation `
  if (props.context) base += `main-navigation--${props.context} `
  return base
})

const visibleNavItems = computed(() => {
  return getCurrentInstance()
      .appContext
      .config
      .globalProperties
      .$clientData
      .mainNavigation
      .filter(o => !o.hide);
});
</script>
