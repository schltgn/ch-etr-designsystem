<template>
  <div
    id="top-bar-container"
    class="top-bar"
    :class="isOpen ? 'top-bar--is-open' : ''"
  >
    <div id="top-bar" :class="computedTopBarClass">
      <div class="container container--flex">
        <button
          v-if="!isEasyLanguage && !isSignLanguage"
          class="top-bar__btn"
          @click="triggerTopBar()"
        >
          <span>{{ $clientData.t('topBar.link.open') }}</span>
          <SvgIcon icon="ChevronDown" size="lg" class="top-bar__btn__icon" />
        </button>
        <div v-else />
        <div class="top-bar__right">
          <Badge
            v-if="isEasyLanguage || isSignLanguage"
            icon="Cancel"
            :iconLeft="computedAccessibilityIcon"
            :label="computedAccessibilityBadgeLabel"
            size="base"
          />
          <TopBarNavigation v-if="!isEasyLanguage && !isSignLanguage" />
          <LanguageSwitcher type="negative" />
        </div>
      </div>
    </div>
    <div v-if="useStickyPlaceholder" id="stickyTopBarPlaceholder" />
    <div v-if="isOpen" class="top-bar__drawer">
      <div class="container">
        <div class="flex justify-end">
          <button class="top-bar__drawer__close__btn" @click="isOpen = !isOpen">
            <span>{{ $clientData.t('topBar.link.close') }}</span>
            <SvgIcon icon="Cancel" size="lg" />
          </button>
        </div>
        <div>
          <h3 class="top-bar__main-title">
            {{ $clientData.t('topBar.youAreHere') }}
          </h3>
          <nav
            aria-label="breadcrumb"
            aria-current="location"
            class="localization"
          >
            <ul>
              <li>
                <a :href="$clientData.t('topBar.department.uvek.url')">{{
                  $clientData.t('topBar.department.uvek.abbr')
                }}</a>
                <SvgIcon icon="ArrowRight" class="localization__icon" />
              </li>
              <li>
                <a
                  :href="$clientData.t('topBar.authority.bafu.url')"
                  class="active"
                  aria-current="page"
                >
                  {{ $clientData.t('topBar.authority.bafu.name') }}
                  {{ $clientData.t('topBar.authority.bafu.abbr') }}
                </a>
              </li>
            </ul>
          </nav>
        </div>

        <div class="separator separator--negative separator--xl" />

        <div>
          <h3 class="top-bar__main-title">
            {{ $clientData.t('topBar.visitAnother') }}
          </h3>

          <div class="top-bar__grid">
            <div class="top-bar__grid__box-1">
              <h4 class="top-bar__title">
                {{ $clientData.t('topBar.government.heading') }}
              </h4>
              <ul class="menu">
                <li
                  v-for="item in government"
                  class="menu__item menu__item--negative menu__item--brim"
                >
                  <a :href="item.url" class="menu__item__flex">
                    <div>
                      <div class="overtitle">{{ item.abbr }}</div>
                      <div>{{ item.name }}</div>
                    </div>
                    <SvgIcon icon="External" class="menu__item__icon" />
                  </a>
                </li>
              </ul>
            </div>
            <div class="top-bar__grid__box-2">
              <!-- Potential additional information here -->
            </div>

            <div class="top-bar__grid__box-3">
              <h4 class="top-bar__title">
                {{ $clientData.t('topBar.department.heading') }}
              </h4>
              <ul class="menu">
                <li
                  v-for="item in departments"
                  class="menu__item menu__item--negative menu__item--brim"
                >
                  <a :href="item.url" class="menu__item__flex">
                    <div>
                      <div class="overtitle">{{ item.abbr }}</div>
                      <div>{{ item.name }}</div>
                    </div>
                    <SvgIcon icon="External" class="menu__item__icon" />
                  </a>
                </li>
              </ul>
            </div>

            <div class="top-bar__grid__box-4">
              <h4 class="top-bar__title">
                {{ $clientData.t('topBar.authority.heading') }}
              </h4>
              <div class="search search--negative search--large">
                <div class="search__group">
                  <input
                    id="search-input"
                    ref="searchInput"
                    v-model="filterString"
                    type="search"
                    label="Ämter filtern"
                    placeholder="Filtern"
                    autocomplete="off"
                  />
                  <Btn
                    v-if="filterString !== ''"
                    label="Clear search input"
                    icon="CancelCircle"
                    icon-pos="only"
                    variant="bare-negative"
                    size="lg"
                    @click="
                      () => {
                        filterString = ''
                        $refs.searchInput.focus()
                      }
                    "
                  />
                  <div v-else class="btn btn--negative btn--lg btn--icon-only">
                    <SvgIcon icon="Filter" class="icon--lg" />
                  </div>
                </div>
                <div class="search__results search__results--negative">
                  <ul class="menu">
                    <li
                      v-for="item in filteredAuthorities"
                      class="menu__item menu__item--negative menu__item--icon-on-hover"
                    >
                      <a :href="item.url" class="menu__item__flex">
                        <div>
                          <div class="overtitle">
                            <span>{{ item.department }}</span>
                            <SvgIcon icon="ArrowRight" class="icon--sm" />
                            <span>{{ item.abbr }}</span>
                          </div>
                          <div>{{ item.name }}</div>
                        </div>
                        <SvgIcon icon="External" class="menu__item__icon" />
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, getCurrentInstance, nextTick, onMounted, ref } from 'vue'
import Badge from '../components/Badge.vue'
import Btn from '../components/Btn.vue'
import LanguageSwitcher from '../components/LanguageSwitcher.vue'
import SvgIcon from '../components/SvgIcon.vue'
import TopBarNavigation from '../navigations/TopBarNavigation.vue'

const filterString = ref('')
const useStickyPlaceholder = ref(false)
const initialTopBarOffset = ref(0)

const isOpen = ref(false)
const props = defineProps({
  isEasyLanguage: {
    type: Boolean,
    default: () => false,
  },
  isSignLanguage: {
    type: Boolean,
    default: () => false,
  },
  isSticky: {
    type: Boolean,
    default: () => false,
  },
})

const computedTopBarClass = computed(() => {
  let base = `top-bar__bar`
  if (props.isEasyLanguage) base += `--easy-language `
  if (props.isSignLanguage) base += `--sign-language `
  return base
})

const computedAccessibilityIcon = computed(() => {
  if (props.isEasyLanguage) {
    return 'EasyLanguage'
  }
  if (props.isSignLanguage) {
    return 'SignLanguage'
  }
  return ''
})

const computedAccessibilityBadgeLabel = computed(() => {
  if (props.isEasyLanguage) {
    return 'Leichte Sprache schliessen'
  }
  if (props.isSignLanguage) {
    return 'Gebärdensprache schliessen'
  }
  return ''
})

const resizeWindow = function () {
  const topBar = document.getElementById('top-bar-container') as HTMLElement
  initialTopBarOffset.value = topBar.offsetTop
  handleScroll()
}

const handleScroll = async function () {
  const topBar = document.getElementById('top-bar') as HTMLElement
  if (window.scrollY > initialTopBarOffset.value) {
    useStickyPlaceholder.value = true
    await nextTick()
    // Set height on placeholder to avoid jump when top bar is set to sticky
    const stickyPlaceholder = document.getElementById(
      'stickyTopBarPlaceholder'
    ) as HTMLElement
    stickyPlaceholder.style.height = `${topBar.clientHeight}px`

    topBar.classList.add('sticky-top-bar')
  } else {
    useStickyPlaceholder.value = false
    topBar.classList.remove('sticky-top-bar')
  }
}

const triggerTopBar = function () {
  isOpen.value = !isOpen.value
  window.postMessage({ trigger: 'top-bar-drawer-change' })
}

const instance = getCurrentInstance();
const t = instance?.appContext.config.globalProperties.$clientData.t ?? (() => '');



const authorities: { url: string; department: string; abbr: string; name: string }[] = [];
t('topBar.authority.keys').split(',').forEach((key: string) => {
  authorities.push({
    url: t('topBar.authority.' + key + '.url'),
    department: t('topBar.authority.' + key + '.department'),
    abbr: t('topBar.authority.' + key + '.abbr'),
    name: t('topBar.authority.' + key + '.name'),
  });
});

const departments: { url: string; abbr: string; name: string }[] = [];
t('topBar.department.keys').split(',').forEach((key: string) => {
  departments.push({
    url: t('topBar.department.' + key + '.url'),
    abbr: t('topBar.department.' + key + '.abbr'),
    name: t('topBar.department.' + key + '.name'),
  });
});

const government: { url: string; abbr: string; name: string }[] = [];
t('topBar.government.keys').split(',').forEach((key: string) => {
  government.push({
    url: t('topBar.government.' + key + '.url'),
    abbr: t('topBar.government.' + key + '.abbr'),
    name: t('topBar.government.' + key + '.name'),
  });
});

const filteredAuthorities = computed(() => {
  return authorities.filter(
    (item) =>
      item.name.toLowerCase().includes(filterString.value.toLowerCase()) ||
      item.department
        .toLowerCase()
        .includes(filterString.value.toLowerCase()) ||
      item.abbr.toLowerCase().includes(filterString.value.toLowerCase())
  )
})

onMounted(() => {
  if (props.isSticky) {
    window.addEventListener('scroll', handleScroll)
    resizeWindow()
    window.addEventListener('resize', resizeWindow)
  }
})
</script>
