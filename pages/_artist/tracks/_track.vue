<template>
  <div>
    <client-only>
      <div
        class="uk-card uk-card-default uk-background-secondary"
        v-if="tracks"
      >
        <div class="uk-card-header">
          <div class="uk-grid-small uk-flex-middle" uk-grid>
            <div class="uk-width-expand">
              <h3 class="uk-card-title uk-margin-remove-bottom">
                {{ tracks[0].title }}
              </h3>
              <p
                class="uk-text-meta uk-margin-remove-top uk-margin-remove-bottom"
                v-for="(artist, key) in tracks[0].artists"
                v-bind:key="key"
              >
                {{ artist.name }}
                <span v-if="artist.jp_name">/{{ artist.jp_name }}</span>
              </p>
            </div>
          </div>
        </div>
        <div class="uk-card-body uk-padding-small">
          <ul uk-tab>
            <li><a href="#" v-if="tracks[0].eng_lyrics">English</a></li>
            <li><a href="#" v-if="tracks[0].jp_lyrics">Japanese</a></li>
            <li><a href="#" v-if="tracks[0].romaji_lyrics">Romaji</a></li>
          </ul>
          <ul class="uk-switcher uk-margin white-space-pre p-20">
            <li v-if="tracks[0].eng_lyrics" v-html="tracks[0].eng_lyrics"></li>
            <li v-if="tracks[0].jp_lyrics" v-html="tracks[0].jp_lyrics"></li>
            <li
              v-if="tracks[0].romaji_lyrics"
              v-html="tracks[0].romaji_lyrics"
            ></li>
          </ul>
        </div>
      </div>
    </client-only>
  </div>
</template>

<script>
import trackQuery from '~/apollo/queries/track/track'

export default {
  data() {
    return {}
  },
  apollo: {
    tracks: {
      prefetch: true,
      query: trackQuery,
      variables() {
        return {
          name: this.$route.params.artist,
          title: this.$route.params.track,
        }
      },
    },
  },
}
</script>
