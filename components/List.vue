<template>
  <div
    class="uk-child-width-1-2@s uk-child-width-1-3@m uk-child-width-1-4@l uk-text-center"
    uk-grid="parallax: 150"
  >
    <div
      class="uk-card uk-card-secondary uk-grid-collapse uk-grid-margin uk-padding-remove-left uk-margin-medium-left"
      v-for="(data, key) in dataList"
      v-bind:class="{ 'uk-child-width-1-2@s': datatype !== 'track' }"
      v-bind:key="key"
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
      <div class="uk-card-body">
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
            v-for="(artist, key) in data.artists"
            class="uk-link"
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
          @click="navigateTo(data, data.artists.name)"
        >
          {{ data.artists.name }}
          <span v-if="data.artists.jp_name">/{{ data.artists.jp_name }}</span>
        </p>
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
            {
              tracks {
                title
                artists {
                  name
                  jp_name
                }
              }
            }
          `
        } else if (this.datatype === 'artist') {
          return gql`
            {
              artists {
                name
                jp_name
                image {
                  url
                }
              }
            }
          `
        } else if (this.datatype === 'album') {
          return gql`
            {
              albums {
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
      update: (data) => data.tracks || data.artists || data.albums,
      skip() {
        return !this.datatype
      },
    },
  },
  methods: {
    navigateTo(data, artist = null) {
      let url = ''
      if (this.datatype === 'track')
        url = '/' + data.artists[0].name + '/tracks/' + data.title
      if (this.datatype === 'album')
        url = '/' + data.artists.name + '/albums/' + data.name
      if (this.datatype === 'artist') url = '/' + data.name
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
