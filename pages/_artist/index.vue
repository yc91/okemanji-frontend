<template>
  <div>
    <client-only>
      <div
        v-if="artists"
        class="uk-card uk-card-default uk-background-secondary"
      >
        <div class="uk-card-header">
          <div class="uk-grid-small uk-flex-middle" uk-grid>
            <div class="uk-width-auto">
              <img
                :data-src="artists[0].image.url"
                width="150"
                height="150"
                uk-img
              />
            </div>
            <div class="uk-width-expand">
              <h3 class="uk-card-title uk-margin-remove-bottom">
                {{ artists[0].name }}
                <span v-if="artists[0].jp_name">/{{ artists[0].jp_name }}</span>
              </h3>
            </div>
          </div>
        </div>
        <div class="uk-card-body">
          <p>
            <a
              class="uk-button uk-button-text"
              :href="artists[0].youtube"
              target="_blank"
              >Youtube</a
            >
          </p>
          <p>
            <a
              class="uk-button uk-button-text"
              :href="artists[0].spotify"
              target="_blank"
              >Spotify</a
            >
          </p>
        </div>
        <div class="uk-card-footer">
          <p>
            <router-link to="albums" class="uk-button uk-button-text" append>
              View Albums
            </router-link>
          </p>
          <p>
            <router-link to="tracks" class="uk-button uk-button-text" append>
              View Tracks
            </router-link>
          </p>
        </div>
      </div>
    </client-only>
  </div>
</template>

<script>
import artistQuery from '~/apollo/queries/artist/artist'

export default {
  data() {
    return {}
  },
  apollo: {
    artists: {
      prefetch: true,
      query: artistQuery,
      variables() {
        return {
          name: this.$route.params.artist,
        }
      },
    },
  },
}
</script>
