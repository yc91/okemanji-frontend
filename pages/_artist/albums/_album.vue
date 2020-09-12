<template>
  <div>
    <client-only>
      <div v-if="albums">
        <div
          class="uk-card uk-card-default uk-grid-collapse uk-background-secondary uk-margin"
          uk-grid
        >
          <div class="uk-card-media-left uk-cover-container">
            <img :src="albums[0].image.url" alt="" uk-cover />
            <canvas width="600" height="400"></canvas>
          </div>
          <div>
            <div class="uk-card-body">
              <h3 class="uk-card-title">{{ albums[0].name }}</h3>
              <a class="uk-text-meta" :href="'/' + albums[0].artists.name">
                {{ albums[0].artists.name }}
                <span v-if="albums[0].artists.jp_name"
                  >/{{ albums[0].artists.jp_name }}</span
                >
              </a>
            </div>
          </div>
        </div>
        <table class="uk-table uk-table-divider">
          <thead>
            <tr>
              <th class="uk-text-primary">Track</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(track, key) in albums[0].tracks" v-bind:key="key">
              <td>
                <a
                  :href="
                    '/' + albums[0].artists.name + '/tracks/' + track.title
                  "
                  >{{ track.title }}</a
                >
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </client-only>
  </div>
</template>

<script>
import albumQuery from '~/apollo/queries/album/album'

export default {
  data() {
    return {}
  },
  apollo: {
    albums: {
      prefetch: true,
      query: albumQuery,
      variables() {
        return {
          name: this.$route.params.artist,
          title: this.$route.params.album,
        }
      },
    },
  },
}
</script>
