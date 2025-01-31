<template>
  <li>
    <a
      :href="navItem.url ? navItem.url : '#'"
      :class="{
        'breadcrumb__has-children': navItem.children.length > 0,
        active: true,
      }"
      :aria-current="true"
    >
      <SvgIcon
        icon="ChevronRight"
        class="breadcrumb__include-icon"
        aria-hidden="true"
      />
      <span>{{ navItem.text }}</span>
      <SvgIcon
        icon="ChevronDown"
        class="breadcrumb__dropdown-icon"
        aria-hidden="true"
        v-if="navItem.children.length > 0"
      />
    </a>
    <ul v-if="navItem.children.length > 0">
      <li v-for="child in navItem.children">
        <a :href="child.url"
           :class="{ active: child.active }"
           :aria-current="child.active">
          <span>{{ child.text }}</span>
        </a>
      </li>
    </ul>
  </li>
</template>

<script setup>
import SvgIcon from '../components/SvgIcon.vue'

const props = defineProps({
  navItem: { type: Object },
  level: { type: Number },
  index: { type: Number },
})
</script>
