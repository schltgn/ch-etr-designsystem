<template>
  <nav :class="breadcrumbNavigationClass" aria-label="Breadcrumb">
    <ul>
      <li>
        <a :href="$clientData.contextPath">
          <span>Startseite</span>
        </a>
      </li>
      <BreadcrumbNavigationItem
          v-for="(navItem, index) in breadcrumbNavItems"
          :navItem="navItem"
          :level="0"
          :index="index"
          :context="context"
      />
    </ul>
  </nav>
</template>

<script setup>
import BreadcrumbNavigationItem from './BreadcrumbNavigationItem.vue'
import { computed, getCurrentInstance } from 'vue'

const props = defineProps({
  isSimplePage: {
    type: Boolean,
    default: () => false,
  },
  context: {
    type: String,
    default: () => '',
  },
})

const breadcrumbNavigationClass = computed(() => {
  let base = `breadcrumb-navigation `
  if (props.context) base += `breadcrumb-navigation--${props.context} `
  return base
})

const breadcrumbNavItems = computed(() => {
  const items = findBreadcrumbs(
      getCurrentInstance()
          .appContext
          .config
          .globalProperties
          .$clientData
          .mainNavigation
  );
  return items;
});

const findBreadcrumbs = (navItems, path = []) => {
  for (const item of navItems) {
    if (isActive(item)) {
      const newPath = [...path, item];
      if (item.children && item.children.length) {
        const childPath = findBreadcrumbs(item.children, newPath);
        if (childPath.length) return childPath;
      }
      return newPath;
    }
  }
  return [];
};

const isActive = (navItem) => {
  if (navItem.active) {
    return true; // If the current item has the property, return true
  }
  // Recursively check children (if they exist)
  return navItem.children?.some(child => isActive(child)) || false;
};

</script>
