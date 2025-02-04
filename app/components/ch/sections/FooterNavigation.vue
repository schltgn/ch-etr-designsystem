<template>
  <div class="bg--secondary-700">
    <nav class="container" aria-label="Footer">
      <ul class="footer-navigation">
        <li
          v-for="(item, index) in $clientData.footerNavigation"
          :class="{
            'mr-auto': index === $clientData.footerNavigation.length - 1,
          }"
        >
          <a :href="item.url" class="footer__link">{{ item.text }}</a>
        </li>
        <li class="footer__label ml-auto" v-if="$clientData.personIdentifier != null">
          {{ $clientData.personIdentifier }}
        </li>
        <li class="footer__label">{{ $clientData.currentDateTime }}</li>
      </ul>
    </nav>
  </div>
</template>
<script setup lang="ts"></script>