<template>
  <div
    class="uk-child-width-1-2@s uk-child-width-1-3@m uk-child-width-1-4@l uk-text-center uk-grid-match"
    uk-grid="parallax: 150"
  >
    <div v-for="(data, key) in dataList" v-bind:key="key">
      <div
        class="uk-card uk-card-secondary uk-grid-collapse uk-grid-margin uk-grid-small"
        v-bind:class="{ 'uk-child-width-1-2@s': datatype !== 'track' }"
        :uk-grid="datatype !== 'track'"
      >
        <div class="uk-card-media-left uk-cover-container" v-if="data.image">
          <img
            :src="data.image.url"
            @click="navigateTo(data)"
            class="cursor-pointer"
            uk-cover
          />
          <canvas width="600" height="400"></canvas>
        </div>
        <div class="uk-padding-small">
          <h3
            class="uk-card-title uk-inline uk-link"
            v-if="data.title"
            @click="navigateTo(data)"
          >
            {{ data.title }}
          </h3>
          <h3
            class="uk-card-title uk-inline uk-link"
            v-if="data.name"
            @click="navigateTo(data)"
          >
            {{ data.name }}<span v-if="data.jp_name">/{{ data.jp_name }}</span>
          </h3>
          <div v-if="data.artists && Array.isArray(data.artists)">
            <p
              class="uk-link"
              v-for="(artist, key) in data.artists"
              v-bind:key="key"
              @click="navigateTo(data, artist.name)"
            >
              {{ artist.name }}
              <span v-if="artist.jp_name">/{{ artist.jp_name }}</span>
            </p>
          </div>
          <p
            v-else-if="data.artists"
            class="uk-link"
            @click="navigateTo(data, artists.name)"
          >
            {{ data.artists.name }}
            <span v-if="data.artists.jp_name">/{{ data.artists.jp_name }}</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import gql from 'graphql-tag'
export default {
  data() {
    return {}
  },
  props: ['datatype'],
  apollo: {
    dataList: {
      query() {
        if (this.datatype === 'track') {
          return gql`
            query getFromArtist($name: String!) {
              tracks(where: { artists: { name: $name } }) {
                title
                artists {
                  name
                  jp_name
                }
              }
            }
          `
        } else if (this.datatype === 'album') {
          return gql`
            query getFromArtist($name: String!) {
              albums(where: { artists: { name: $name } }) {
                name
                image {
                  url
                }
                artists {
                  name
                  jp_name
                }
              }
            }
          `
        }
      },
      update: (data) => data.tracks || data.albums,
      variables() {
        return {
          name: this.$route.params.artist,
        }
      },
      skip() {
        return !this.datatype
      },
    },
  },
  methods: {
    navigateTo(data, artist = null) {
      let url = '/' + this.$route.params.artist

      if (this.datatype === 'track') url += '/tracks/' + data.title
      if (this.datatype === 'album') url += '/albums/' + data.name

      if (artist) {
        url = '/' + artist
      }

      this.$router.push({
        path: url,
      })
    },
  },
}
</script>
