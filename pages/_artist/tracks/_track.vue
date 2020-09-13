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
                <span v-if="tracks[0].jp_title">/{{ tracks[0].jp_title }}</span>
              </h3>
              <p
                class="uk-text-meta uk-margin-remove-top uk-margin-remove-bottom uk-link"
                v-for="(artist, key) in tracks[0].artists"
                v-bind:key="key"
                @click="$router.push({ path: '/' + artist.name })"
              >
                {{ artist.name }}
                <span v-if="artist.jp_name">/{{ artist.jp_name }}</span>
              </p>
              <div class="uk-margin uk-grid-small uk-child-width-auto uk-grid">
                <label
                  ><input
                    class="uk-checkbox"
                    type="checkbox"
                    v-model="showYt"
                    checked
                  />Show Youtube</label
                >
                <label v-if="showYt && tracks[0].off_vocal_youtube !== ''"
                  ><input
                    class="uk-checkbox uk-margin-small-right"
                    v-model="vocal"
                    type="checkbox"
                  />Vocal</label
                >
              </div>
            </div>
          </div>
        </div>
        <div class="uk-card-body uk-padding-small">
          <ul uk-tab>
            <li><a href="#" v-if="tracks[0].romaji_lyrics">Romaji</a></li>
            <li><a href="#" v-if="tracks[0].eng_lyrics">English</a></li>
            <li><a href="#" v-if="tracks[0].jp_lyrics">Japanese</a></li>
          </ul>
          <ul class="uk-switcher uk-margin white-space-pre p-20">
            <li
              class="uk-animation-fade"
              v-if="tracks[0].romaji_lyrics"
              v-html="tracks[0].romaji_lyrics"
            ></li>
            <li
              class="uk-animation-fade"
              v-if="tracks[0].eng_lyrics"
              v-html="tracks[0].eng_lyrics"
            ></li>
            <li
              class="uk-animation-fade"
              v-if="tracks[0].jp_lyrics"
              v-html="tracks[0].jp_lyrics"
            ></li>
          </ul>
          <div class="youtube-embed" v-show="showYt">
            <div v-if="vocal">
              <iframe
                :src="tracks[0].youtube"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen
              ></iframe>
            </div>
            <div v-else>
              <iframe
                :src="tracks[0].off_vocal_youtube"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen
              ></iframe>
            </div>
          </div>
        </div>
      </div>
    </client-only>
  </div>
</template>

<script>
import trackQuery from '~/apollo/queries/track/track'

export default {
  data() {
    return {
      showYt: true,
      vocal: true,
    }
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
