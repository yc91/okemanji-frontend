<template>
  <div>
    <client-only>
      <div class="uk-section">
        <div class="uk-container">
          <div class="uk-card uk-card-default uk-card-hover uk-card-body">
            <h3 class="uk-text-center">Recently Added Tracks</h3>
            <table class="uk-table uk-table-divider">
              <thead>
                <tr>
                  <th class="uk-text-primary">Track Title</th>
                  <th class="uk-text-primary">Artist Name</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="track in tracks" v-bind:key="track.id">
                  <td>
                    <a
                      :href="
                        '/' + track.artists[0].name + '/tracks/' + track.title
                      "
                      >{{ track.title }}</a
                    >
                  </td>
                  <td>
                    <p v-for="(artist, key) in track.artists" v-bind:key="key">
                      <a :href="'/' + artist.name">
                        {{ artist.name
                        }}<span v-if="artist.jp_name"
                          >/{{ artist.jp_name }}</span
                        >
                      </a>
                    </p>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </client-only>
  </div>
</template>
<script>
import last10Tracks from '~/apollo/queries/track/last10Tracks'

export default {
  data() {
    return {}
  },
  apollo: {
    tracks: {
      prefetch: true,
      query: last10Tracks,
    },
  },
  methods: {
    toTrack(artistName, title) {
      const url = '/' + artistName + '/tracks/' + title
      this.$router.push({
        path: url,
      })
    },
  },
}
</script>
