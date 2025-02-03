<template>
  <li>
    <a
      href="#"
      @click.prevent
      class="breadcrumb__has-children active"
      :aria-current="true"
    >
      <SvgIcon
        icon="ChevronRight"
        class="breadcrumb__include-icon"
        aria-hidden="true"
      />
      <span>{{ activeItem.text }}</span>
      <SvgIcon
        icon="ChevronDown"
        class="breadcrumb__dropdown-icon"
        aria-hidden="true"
        v-if="navItems.length > 1"
      />
    </a>
    <ul v-if="visibleNavItems.length > 1">
      <li v-for="item in visibleNavItems">
        <a :href="item.url != null ? item.url : item.children[0].url"
           :class="{ active: isActive(item) }"
           :aria-current="isActive(item)">
          <span>{{ item.text }}</span>
        </a>
      </li>
    </ul>
  </li>
</template>

<script setup>
import SvgIcon from '../components/SvgIcon.vue'
import {computed} from "vue";

const props = defineProps({
  navItems: { type: Array },
})

const activeItem = computed(() => {
  return props.navItems.find((item) => isActive(item));
});

const visibleNavItems = computed(() => {
  return props.navItems.filter((item) => !item.hide);
});

const isActive = (navItem) => {
  if (navItem.active) {
    return true; // If the current item has the property, return true
  }
  // Recursively check children (if they exist)
  return navItem.children?.some(child => isActive(child)) || false;
};
</script>
