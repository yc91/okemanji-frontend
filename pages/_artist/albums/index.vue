<template>
  <div>
    <client-only>
      <div>
        <div class="uk-container">
          <div class="uk-card">
            <h3 class="uk-text-center header-spacing">Albums</h3>
            <ListOne datatype="album" />
          </div>
        </div>
      </div>
    </client-only>
  </div>
</template>

<script>
import ListOne from '~/components/ListOne.vue'

export default {
  components: {
    ListOne,
  },
}
</script>
