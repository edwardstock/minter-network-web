<template>
  <div id="nuxt-press" class="docs main-wrap main-wrap--docs">
    <top-header :isMenuActive.sync="isMenuActive"/>
    <div class="main-content u-grid u-grid--no-margin">

      <sidebar class="u-cell u-cell--medium--1-4" :isShowSidebar="isShowSidebar"/>
      <main class="docs-content u-cell u-cell--medium--3-4">
        <nuxt
                class="u-container u-container--content wysiwyg"
        />
      </main>

    </div>

  </div>
</template>

<script>
import debounce from 'lodash-es/debounce';
import {support} from '~/assets/utils-support.js';
import eventBus from '~/assets/event-bus.js';
import docsMixin from 'press/docs/mixins/docs'
import TopHeader from 'press/docs/components/header'
import sidebar from 'press/docs/components/sidebar'

export default {
  components: { TopHeader, sidebar },
  mixins: [docsMixin],
  data() {
    return {
      isMenuActive: false,
      isDesktop: isDesktop(),
    }
  },
  head() {
    // TODO:
    // Try to better understand why setting this in components/topic.vue
    // (often) triggers a 'flash' while switching page/locale
    // due to momentarily having all the head info being removed
    //
    // Its probably often and not always because vue-meta tries to group
    // fast-occuring metaInfo updates which sometimes succeeeds in combining
    // two update triggers and sometimes not
    //
    // Setting keep-alive: true on <nuxt/> seems to improve this slightly, but
    // sometimes the source.vue component still gets re-created and that still
    // triggers the flash
    //
    // To trigger the flash:
    // - remove keep-alive prop from <nuxt/> and set htmmlAttrs in topic.vue
    //
    // To fix? the flash
    // - return htmlAttrs from layout instead of topic
    //
    return {
      htmlAttrs: {
        class: 'docs layout--docs'
      }
    }
  },
  computed: {
    isShowSidebar() {
      return this.isMenuActive || this.isDesktop;
    }
  },
  watch: {
    '$route'(to, from) {
      this.isMenuActive = false;
    },
    isMenuActive(newVal) {
      if (newVal) {
        // pause scroll observer when mobile menu is opened
        eventBus.$emit('pause-observer');
      } else {
        eventBus.$emit('continue-observer');
      }
    }
  },
  mounted() {
    checkHeaderHeight();
    setHeaderTopProperty();
    window.addEventListener('scroll', setHeaderTopProperty, support.passiveListener ? {passive: true} : false);
    window.addEventListener('resize', this.handleResize, support.passiveListener ? {passive: true} : false);
    window.addEventListener('orientationchange', this.handleResize, support.passiveListener ? {passive: true} : false);

  },
  destroyed() {
    window.removeEventListener('scroll', setHeaderTopProperty);
    window.removeEventListener('resize', this.handleResize);
    window.removeEventListener('orientationchange', this.handleResize);
  },
  methods: {
    handleResize() {
      const clientWidth = document.body.clientWidth
      checkHeaderHeight(clientWidth);
      checkDesktop.call(this, clientWidth);
      debouncedSetHeaderTopProperty();
    },
  },
}




const debouncedSetHeaderTopProperty = debounce(setHeaderTopProperty, 50);
function setHeaderTopProperty() {
  document.documentElement.style.setProperty('--header-visible-height', Math.max(headerHeight - window.scrollY, 0) + 'px');
}
let headerHeight = 80;
function checkHeaderHeight(clientWidth) {
  if (!process.client) {
    return;
  }
  clientWidth = clientWidth || document.body.clientWidth;
  if (clientWidth >= 960) {
    headerHeight = 80
  } else {
    headerHeight = 56;
  }
}


function checkDesktop(clientWidth) {
  this.isDesktop = isDesktop(clientWidth);
  if (this.isDesktop) {
    this.isMenuActive = false;
  }
}

function isDesktop(clientWidth) {
  if (!process.client) {
    return;
  }
  clientWidth = clientWidth || document.body.clientWidth;
  return clientWidth >= 700;
}
</script>
