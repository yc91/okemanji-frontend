<template>
  <div>
    <client-only>
      <div class="uk-section">
        <div class="uk-container">
          <div class="uk-card uk-card-body">
            <h3 class="uk-text-center">Tracks</h3>
            <ListOne datatype="track" />
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
