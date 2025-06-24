<template>
  <div class="bg--secondary-700">
    <nav class="container" aria-label="Footer">
      <ul class="footer-navigation">
        <li
          v-for="(item, index) in $clientData.footer2Navigation"
          :class="{
            'mr-auto': index === $clientData.footer2Navigation.length - 1,
          }"
        >
          <a :href="item.url" class="footer__link">{{ item.text }}</a>
        </li>
        <li class="footer__label ml-auto">{{ $clientData.currentDateTime }}</li>
        <li class="footer__label" v-if="$clientData.personIdentifier != null">
          {{ $clientData.personIdentifier }}
        </li>
      </ul>
    </nav>
  </div>
</template>
<script setup lang="ts">
</script>