<template>
  <div>
    <client-only>
      <div>
        <div class="uk-container">
          <div class="uk-card">
            <h3 class="uk-text-center header-spacing">Artists</h3>
            <List datatype="artist" />
          </div>
        </div>
      </div>
    </client-only>
  </div>
</template>

<script>
import List from '~/components/List.vue'

export default {
  components: {
    List,
  },
}
</script>
