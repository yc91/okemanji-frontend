<template>
  <div>
    <client-only>
      <div>
        <div class="uk-container">
          <h3 class="uk-text-center header-spacing">
            Explore more of the site here
          </h3>
          <div
            class="uk-child-width-1-3@m uk-margin-remove uk-grid-margin uk-grid-small uk-grid-match"
            uk-grid
          >
            <div v-for="(card, key) in explore" v-bind:key="key">
              <div
                class="uk-card uk-card-secondary cursor-pointer"
                @click="$router.push({ path: card.url })"
              >
                <div class="uk-card-media-top">
                  <div class="uk-text-center">
                    <div
                      class="uk-inline-clip uk-transition-toggle uk-light"
                      tabindex="0"
                    >
                      <img :src="card.image" />
                      <div
                        class="uk-position-cover uk-overlay-primary uk-flex uk-flex-center uk-flex-middle uk-transition-fade"
                      >
                        <div class="uk-position-center">
                          <div class="uk-transition-slide-top-small">
                            <h4 class="uk-margin-remove">EXPLORE</h4>
                          </div>
                          <div class="uk-transition-slide-bottom-small">
                            <h4 class="uk-margin-remove">{{ card.title }}</h4>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="uk-card-body uk-text-center">
                  <h3 class="uk-card-title">{{ card.title }}</h3>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="uk-container uk-margin-medium-top">
          <div
            class="uk-card uk-card-secondary uk-card-hover uk-card-body"
            style="margin-left: 16px"
          >
            <h3 class="uk-text-center header-spacing">Recently Added Tracks</h3>
            <table class="uk-table uk-table-divider">
              <thead>
                <tr>
                  <th>Track Title</th>
                  <th>Artist Name</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="track in tracks" v-bind:key="track.id">
                  <td>
                    <a
                      href="#"
                      @click="
                        $router.push({
                          path:
                            '/' +
                            track.artists[0].name +
                            '/tracks/' +
                            track.title,
                        })
                      "
                      >{{ track.title }}</a
                    >
                  </td>
                  <td>
                    <p v-for="(artist, key) in track.artists" v-bind:key="key">
                      <a
                        href="#"
                        @click="
                          $router.push({
                            path: '/' + artist.name,
                          })
                        "
                      >
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
import last10Tracks from '~/apollo/queries/track/last10tracks'

export default {
  data() {
    return {
      explore: [
        {
          title: 'Artists',
          image: 'images/pexels-wendy-wei-1916818.jpg',
          url: '/artists',
        },
        {
          title: 'Albums',
          image: 'images/pexels-dids-1616470.jpg',
          url: '/albums',
        },
        {
          title: 'Tracks',
          image: 'images/pexels-mati-mango-4734716.jpg',
          url: '/tracks',
        },
      ],
    }
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
