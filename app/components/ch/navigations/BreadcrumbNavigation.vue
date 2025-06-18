<template>
  <nav :class="breadcrumbNavigationClass" aria-label="Breadcrumb">
    <ul>
      <li>
        <a :href="$clientData.contextPath">
          <span>{{ $clientData.t('form.menu.home') }}</span>
        </a>
      </li>
      <BreadcrumbNavigationItem
          v-for="(navItems) in breadcrumbNavItems"
          :navItems="navItems"
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
  const activeItem = navItems.find(child => isActive(child)) || false;
  if (activeItem) {
    const newPath = [...path, navItems];
    return findBreadcrumbs(activeItem.children, newPath);
  }
  return path;
};

const isActive = (navItem) => {
  if (navItem.active) {
    return true; // If the current item has the property, return true
  }
  // Recursively check children (if they exist)
  return navItem.children?.some(child => isActive(child)) || false;
};

</script>
