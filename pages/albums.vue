<template>
  <div>
    <client-only>
      <div>
        <div class="uk-container">
          <div class="uk-card">
            <h3 class="uk-text-center header-spacing">Albums</h3>
            <List datatype="album" />
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
