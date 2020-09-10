<template>
  <div>
    <table class="uk-table uk-table-divider">
      <thead>
        <tr v-if="datatype === 'artist'">
          <th></th>
          <th class="uk-text-primary">Artist</th>
        </tr>
        <tr v-else-if="datatype === 'album'">
          <th></th>
          <th class="uk-text-primary">Album</th>
          <th class="uk-text-primary">Artists</th>
        </tr>
        <tr v-else-if="datatype === 'track'">
          <th class="uk-text-primary">Track</th>
          <th class="uk-text-primary">Artists</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, key) in dataList" v-bind:key="key">
          <td v-if="data.image">
            <img
              class="uk-border-circle cursor-pointer"
              width="150"
              height="150"
              @click="navigateTo(data)"
              :src="endPointUrl + data.image.url"
            />
          </td>
          <td v-if="data.title">
            <span class="uk-link-heading uk-link" @click="navigateTo(data)">{{
              data.title
            }}</span>
          </td>
          <td v-if="data.name" class="uk-link-toggle">
            <p class="uk-link" @click="navigateTo(data)">
              <span>{{ data.name }}</span>
              <span v-if="data.jp_name">/{{ data.jp_name }}</span>
            </p>
          </td>
          <td v-if="data.artists && Array.isArray(data.artists)">
            <p
              class="uk-text-meta uk-margin-remove-top uk-link"
              v-for="(artist, key) in data.artists"
              v-bind:key="key"
              @click="navigateTo(data, artist.name)"
            >
              {{ artist.name }}
              <span
                class="uk-link"
                v-if="artist.jp_name"
                @click="navigateTo(data, artist.name)"
                >/{{ artist.jp_name }}</span
              >
            </p>
          </td>
          <td v-else-if="data.artists">
            <p
              class="uk-text-meta uk-margin-remove-top uk-link"
              @click="navigateTo(data, data.artists.name)"
            >
              {{ data.artists.name }}
              <span
                class="uk-link"
                v-if="data.artists.jp_name"
                @click="navigateTo(data, data.artists.name)"
                >/{{ data.artists.jp_name }}</span
              >
            </p>
          </td>
        </tr>
      </tbody>
    </table>
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
        url = '/' + data.artists[0] + '/tracks/' + data.title
      if (this.datatype === 'album')
        url = '/' + data.artists[0] + '/albums/' + data.name
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
