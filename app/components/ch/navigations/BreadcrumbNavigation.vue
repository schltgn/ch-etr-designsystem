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
      <!--
      <li>
        <a
          href="javascript:void(0)"
          class="breadcrumb__has-children active"
          aria-current="true"
        >
          <SvgIcon
            icon="ChevronRight"
            class="breadcrumb__include-icon"
            aria-hidden="true"
          />
          <span>Diensleistungen</span>
          <SvgIcon
            icon="ChevronDown"
            class="breadcrumb__dropdown-icon"
            aria-hidden="true"
          />
        </a>
        <ul>
          <li>
            <a href="javascript:alert('link')">
              <span>News</span>
            </a>
          </li>
          <li>
            <a href="javascript:alert('link')">
              <span>Geodaten</span>
            </a>
          </li>
          <li>
            <a href="javascript:alert('link')">
              <span>Karten</span>
            </a>
          </li>
          <li>
            <a href="javascript:alert('link')">
              <span>Geoportale</span>
            </a>
          </li>
          <li>
            <a
              href="javascript:alert('link')"
              class="active"
              aria-current="true"
            >
              <span>Dienstleistungen</span>
            </a>
          </li>
          <li>
            <a href="javascript:alert('link')">
              <span>Forschung und Lehre</span>
            </a>
          </li>
          <li>
            <a href="javascript:alert('link')">
              <span>Über geo.admin.ch</span>
            </a>
          </li>
        </ul>
      </li>
      <li>
        <a href="#">
          <SvgIcon
            icon="ChevronRight"
            class="breadcrumb__include-icon"
            aria-hidden="true"
          />
          <span>Geodienste</span>
          <SvgIcon
            icon="ChevronDown"
            class="breadcrumb__dropdown-icon"
            aria-hidden="true"
          />
        </a>
        <ul>
          <li>
            <a href="javascript:alert('link')"
              >Datenmodellablage: Model Repository für Geobasisdaten des
              Bundesrechts</a
            >
          </li>
          <li>
            <a
              href="javascript:alert('link')"
              class="active"
              aria-current="true"
              >Geodienste: Informationen zugänglich machen und vernetzen</a
            >
          </li>
          <li>
            <a href="javascript:alert('link')"
              >Datenbezug: einfach und direkt</a
            >
          </li>
          <li>
            <a href="javascript:alert('link')"
              >Beratung und Koordination: Umsetzen der Strategie</a
            >
          </li>
          <li><a href="javascript:alert('link')">RSS und Social Media</a></li>
        </ul>
      </li>
      <li>
        <a href="#" aria-current="true">
          <SvgIcon
            icon="ChevronRight"
            class="breadcrumb__include-icon"
            aria-hidden="true"
          />
          <span>Darstellungsdienste</span>
          <SvgIcon
            icon="ChevronDown"
            class="breadcrumb__dropdown-icon"
            aria-hidden="true"
          />
        </a>
        <ul>
          <li><a href="javascript:alert('link')">Download-Dienste</a></li>
          <li>
            <a
              href="javascript:alert('link')"
              class="active"
              aria-current="true"
              >Darstellungsdienste</a
            >
          </li>
          <li>
            <a href="javascript:alert('link')"
              >Linked Data Dienst: GeoDaten semantisch verlinken</a
            >
          </li>
          <li>
            <a href="javascript:alert('link')"
              >Nutzungsbedingungen und Betriebsbestimmungen der Infrastruktur</a
            >
          </li>
          <li><a href="javascript:alert('link')">Suchdienst CSW</a></li>
          <li>
            <a href="javascript:alert('link')"
              >Konformitätsprüfung Geobasisdienste</a
            >
          </li>
          <li><a href="javascript:alert('link')">INSPIRE Dienste</a></li>
        </ul>
      </li>
      <li>
        <a href="#" aria-current="true">
          <SvgIcon
            icon="ChevronRight"
            class="breadcrumb__include-icon"
            aria-hidden="true"
          />
          <span>Web Map Services</span>
          <SvgIcon
            icon="ChevronDown"
            class="breadcrumb__dropdown-icon"
            aria-hidden="true"
          />
        </a>
        <ul>
          <li>
            <a
              href="javascript:alert('link')"
              class="active"
              aria-current="page"
              >Web Map Services</a
            >
          </li>
          <li><a href="javascript:alert('link')">Web tiling Services</a></li>
          <li><a href="javascript:alert('link')">Vector Tiles Service</a></li>
          <li>
            <a href="javascript:alert('link')">Web Integration: iFrame</a>
          </li>
          <li><a href="javascript:alert('link')">FAQ API</a></li>
        </ul>
      </li>
      -->
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
  console.log("Active navigation items ", items);
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
