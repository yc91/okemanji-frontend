<template>
  <div>
    <Header />

    <div class="uk-section uk-section-default">
      <div class="uk-container uk-container-large">
        <nuxt />
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import global from '~/mixins/global.js'
// Import your new Header component
import Header from '~/components/Header.vue'
Vue.mixin(global)
export default {
  components: {
    Header,
  },
}
</script>
