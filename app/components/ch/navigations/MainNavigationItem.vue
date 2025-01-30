<template>
  <li>
    <a v-if="level > 0" @click.prevent role="button" class="navy__back">
      <SvgIcon icon="ArrowLeft" size="lg" />
      <span>Back</span>
    </a>
    <h2 v-if="level > 0 && index == 0" class="navy__title">{{ navItem.text }}</h2>
    <a
        :href="navItem.url ? navItem.url : '#'"
        :role="navItem.children.length > 0 ? 'button' : null"
        :class="{'navy__has-children' : navItem.children.length > 0, active : showActiveNavigation && navItem.active }"
        :aria-current="showActiveNavigation && navItem.active"
    >
      <span>{{ navItem.text }}</span>
      <SvgIcon v-if="navItem.children.length > 0 && context == 'mobile'" icon="ArrowRight" size="lg" />
    </a>
    <ul v-if="navItem.children.length > 0">
        <MainNavigationItem
            v-for="(child, index) in navItem.children"
            :navItem="child"
            :context="context"
            :level="level+1"
            :index="index"
            :showActiveNavigation="showActiveNavigation"
        />
    </ul>
  </li>
</template>

<script setup lang="ts">
import MainNavigationItem from './MainNavigationItem.vue'
import SvgIcon from '../components/SvgIcon.vue'

defineProps({
  navItem: { type: Object },
  level: { type: Number },
  index: { type: Number },
  context: { type: String },
  showActiveNavigation: { type: Boolean },
})
</script>
