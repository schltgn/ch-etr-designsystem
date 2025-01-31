<template>
  <nav
    id="main-navigation"
    class="main-navigation main-navigation--desktop"
    aria-label="Main"
  >
    <ul class="menu-desktop">
      <li v-for="navItem in $clientData.mainNavigation">
        <a :href="navItem.url">
          <span>{{ navItem.text }}</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script setup lang="ts"></script>
