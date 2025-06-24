<template>
  <div class="bg--secondary-600">
    <div class="container">
      <div class="footer-information">
        <div class="footer-information__entry">
          <h3 v-html="$clientData.t('footer.contact.heading')" />
          <p v-html="$clientData.t('layout.footer')" />
        </div>

        <div class="footer-information__entry">
          <h3>{{ $clientData.t('layout.footer.headingLinks') }}</h3>
          <div class="footer-information__links">
            <div class="footer-information__links-column">
              <a
                v-for="(item) in $clientData.footer1Navigation"
                :href="item.url"
                class="footer__link footer-information__link--icon-right"
              >
                <SvgIcon icon="ArrowRight" class="footer-information__icon" />
                {{ item.text }}
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import SvgIcon from '../components/SvgIcon.vue'
</script>
