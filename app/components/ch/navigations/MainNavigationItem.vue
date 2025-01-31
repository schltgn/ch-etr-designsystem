<template>
  <li>
    <a v-if="level > 0" @click.prevent role="button" class="navy__back">
      <SvgIcon icon="ArrowLeft" size="lg" />
      <span>Back</span>
    </a>
    <h2 v-if="parentNavItem != null && index == 0" class="navy__title">{{ parentNavItem.text }}</h2>
    <a
        :href="navItem.url ? navItem.url : '#'"
        :role="navItem.children.length > 0 ? 'button' : null"
        :class="{'navy__has-children' : navItem.children.length > 0, active : showActiveNavigation && isActive }"
        :aria-current="showActiveNavigation && isActive"
    >
      <span>{{ navItem.text }}</span>
      <SvgIcon v-if="navItem.children.length > 0 && context == 'mobile'" icon="ArrowRight" size="lg" />
    </a>
    <ul v-if="navItem.children.length > 0">
        <MainNavigationItem
            v-for="(child, index) in navItem.children"
            :nav-item="child"
            :context="context"
            :level="level+1"
            :index="index"
            :show-active-navigation="showActiveNavigation"
            :parent-nav-item="navItem"
        />
    </ul>
  </li>
</template>

<script setup>
import SvgIcon from '../components/SvgIcon.vue'
import { computed } from "vue";

// Define props without type annotations
const props = defineProps({
  navItem: { type: Object },
  parentNavItem: { type: Object },
  level: { type: Number },
  index: { type: Number },
  context: { type: String },
  showActiveNavigation: { type: Boolean },
})

// Recursive function to check if the item or any child has the property
const computeIsActive = (navItem) => {
  if (navItem.active) {
    return true; // If the current item has the property, return true
  }
  // Recursively check children (if they exist)
  return navItem.children?.some(child => computeIsActive(child)) || false;
};

// Computed property for template binding
const isActive = computed(() => computeIsActive(props.navItem));

</script>
